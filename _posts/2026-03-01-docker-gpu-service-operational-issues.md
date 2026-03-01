---
layout: post
title: "Docker GPU 서비스 운영에서 만난 실전 이슈들"
subtitle: "디스크 부족, Healthcheck 타이밍, 모델 캐시 전략"
tags: [MLOps & Infra]
---

## 배경

BERT 기반 NER 서비스를 Docker 컨테이너로 운영하면서 만난 인프라 수준의 이슈들을 정리한다. 코드 로직이 아닌 운영 환경에서 발생하는 문제들이라 디버깅이 어렵고, 한번 겪으면 다음에도 반복되기 쉬운 것들이다.

---

## 1. Docker 디스크 부족

### 증상

빌드 또는 배포 중 디스크 부족 에러 발생. 서버에 충분한 디스크가 있다고 생각했지만, Docker가 내부적으로 사용하는 공간이 누적되어 있었다.

### 원인

Docker 이미지, 볼륨, 빌드 캐시가 시간이 지남에 따라 누적된다. 특히 GPU 모델 서비스는 이미지 크기가 수 GB에 달하고, 빌드를 반복할수록 중간 레이어 캐시가 쌓인다.

### 해결

```bash
docker system prune -a --volumes -f
```

이 명령으로 **62GB**를 회수했다. 사용하지 않는 이미지, 중지된 컨테이너, 미사용 볼륨, 빌드 캐시를 모두 정리한다.

### 예방

- 주기적으로 `docker system df`로 사용량을 모니터링
- CI/CD 파이프라인에 정리 단계를 포함
- 빌드 시 `--no-cache`를 남용하지 않고, 레이어 캐시를 효율적으로 활용하는 Dockerfile 작성

---

## 2. Healthcheck 타이밍과 모델 로딩

### 증상

컨테이너가 시작 후 `unhealthy` 상태로 판정되어 재시작 루프에 빠짐.

### 원인

NER 서비스는 시작 시 BERT 모델 다운로드와 GPU 워밍업이 필요하다. 이 과정이 **87초** 소요되는데, Docker healthcheck의 `start_period`가 **60초**로 설정되어 있어 모델 로딩이 완료되기 전에 healthcheck가 시작되었다.

```yaml
# 문제가 된 설정
healthcheck:
  test: ["CMD", "wget", "-q", "--spider", "http://localhost:8002/health"]
  interval: 30s
  timeout: 10s
  retries: 3
  start_period: 60s  # 모델 로딩 87초보다 짧음
```

### 해결

**1) start_period 상향**

```yaml
start_period: 120s  # 모델 로딩 시간 + 충분한 여유
```

**2) 모델 캐시를 Named Volume으로 마운트**

```yaml
volumes:
  ner-models:

services:
  ner-service:
    volumes:
      - ner-models:/root/.cache/models
```

Named volume으로 모델 파일을 영속화하면, 컨테이너 재시작 시 모델 다운로드를 스킵할 수 있다. 87초 → 수 초로 시작 시간이 단축된다.

### 교훈

- `start_period`는 서비스의 **최악의 경우** 시작 시간보다 충분히 길어야 한다
- 대용량 모델 파일은 이미지에 포함하지 말고 별도 볼륨으로 관리하면 빌드 시간과 시작 시간 모두 개선된다
- 초기 배포 시 모델 다운로드가 필요하므로, 첫 배포의 start_period는 더 길게 설정하는 것도 방법이다

---

## 3. Alpine wget의 IPv6 해석 이슈

### 증상

Docker healthcheck가 실패하지만, 컨테이너에 직접 접속하여 curl로 요청하면 정상 응답.

### 원인

Alpine Linux의 `wget`이 `localhost`를 IPv6 주소(`::1`)로 해석했다. 서비스가 IPv4(`0.0.0.0`)에서만 리스닝하고 있으므로 연결이 거부됐다.

```yaml
# 실패하는 설정
test: ["CMD", "wget", "-q", "--spider", "http://localhost:8002/health"]

# 동작하는 설정
test: ["CMD", "wget", "-q", "--spider", "http://127.0.0.1:8002/health"]
```

### 교훈

컨테이너 환경에서 `localhost`는 IPv4(`127.0.0.1`)와 IPv6(`::1`) 모두로 해석될 수 있다. Healthcheck에서는 명시적 IP 주소를 사용하는 것이 안전하다. 이 이슈는 Alpine 기반 이미지에서 특히 빈번하다.

---

## 정리

| 이슈 | 증상 | 핵심 원인 | 해결 |
|---|---|---|---|
| 디스크 부족 | 빌드/배포 실패 | 이미지/볼륨/캐시 누적 | `docker system prune` + 모니터링 |
| Healthcheck 실패 | unhealthy 재시작 루프 | start_period < 모델 로딩 시간 | start_period 상향 + named volume 캐시 |
| wget IPv6 | healthcheck 연결 거부 | localhost → ::1 해석 | 127.0.0.1 명시 |

이런 운영 이슈들은 코드 리뷰에서 잡히지 않고, 실제 배포 환경에서만 드러난다. 한번 정리해두면 같은 삽질을 반복하지 않을 수 있다.
