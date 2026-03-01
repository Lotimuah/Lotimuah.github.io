---
layout: post
title: "NER API 서버 아키텍처: Uvicorn-Nginx-MySQL 계층별 설정 균형"
subtitle: "동시 요청 50건에서 드러난 계층 간 병목과 설정 간 균형 원칙"
tags: [Architecture]
---

## 배경

NER API 서비스에 50건의 동시 요청을 보내는 부하 테스트에서 반복적으로 실패가 발생했다. 개별 계층(Uvicorn, Nginx, MySQL)의 설정은 각각 합리적이었지만, **계층 간 설정의 불균형**이 병목을 만들고 있었다. 이 글에서는 각 계층의 설정 의미와 계층 간 균형 원칙을 정리한다.

---

## 1. Uvicorn 서버 설정

### 기본값의 문제

초기 상태는 `uvicorn ... --host 0.0.0.0 --port 8002`로 기본값만 사용하고 있었다. Uvicorn 기본 설정은 `--backlog 128`, `--limit-concurrency` 무제한이다. 동시 요청이 폭주하면 연결 거부 또는 OOM이 발생할 수 있다.

### 적용한 설정

```
--backlog 256          # TCP 대기 큐 확대
--limit-concurrency 100  # 최대 동시 연결 제한
--timeout-keep-alive 30  # 유휴 연결 정리
```

### limit-concurrency의 동작 원리

`limit-concurrency`는 Uvicorn이 동시에 처리하는 요청 수의 상한이다. 초과 시 `503 Service Unavailable`을 **즉시** 반환한다.

**초기 실패 사례:** 50으로 설정했을 때 50건 동시 배치에서 16건이 503으로 실패(34/50 성공). 헬스체크, 연결 타이밍 등이 슬롯을 점유하여 50건이 딱 맞지 않았다. 100으로 상향하여 해결.

값이 너무 높으면 메모리 과다 사용 위험이 있으므로 실제 트래픽 패턴에 맞춰 설정해야 한다.

---

## 2. MySQL Connection Pool

### 매 요청마다 새 연결을 생성하는 비용

기존 코드는 매 요청마다 `mysql.connector.connect()`로 새 TCP 연결을 생성/해제하고 있었다. 요청당 2회(캐시 확인 + 저장) 발생하여 불필요한 TCP 핸드셰이크와 인증 오버헤드가 누적됐다.

### Connection Pool 도입

```python
from mysql.connector.pooling import MySQLConnectionPool

pool = MySQLConnectionPool(pool_size=5, ...)

@contextmanager
def _connection():
    conn = pool.get_connection()
    try:
        yield conn
    finally:
        conn.close()  # 풀에 반환
```

`_connection()` 컨텍스트 매니저에서 풀에서 꺼내 사용 후 반환하는 구조로, `MYSQL_POOL_SIZE` 환경변수로 조정 가능하게 했다.

요청 수가 적으면 체감 효과가 작지만, 동시 요청이 많아질수록 연결 생성 비용이 누적되므로 풀링은 기본 적용이 바람직하다.

---

## 3. Nginx Reverse Proxy

### 직접 노출의 문제

NER 컨테이너 포트를 `ports: "8002:8002"`로 직접 외부에 노출하고 있었다. 연결 풀링, 요청 버퍼링, 타임아웃 분리가 없었고, 향후 수평 확장도 불가능했다.

### Nginx 프록시 도입

`ner-lb` 컨테이너를 추가하고 NER 서비스를 `expose`(내부 전용)로 변경했다.

```nginx
upstream ner_backend {
    server ner-service:8002;
    keepalive 64;
}

server {
    proxy_connect_timeout 10s;
    proxy_send_timeout 60s;
    proxy_read_timeout 300s;
}
```

- **keepalive 64:** Nginx가 백엔드로 유지하는 유휴 연결 수
- **타임아웃 분리:** connect(10s), send(60s), read(300s)로 각 단계별 적절한 값 설정
- **수평 확장:** GPU 추가 시 upstream에 서버 1줄 추가로 로드밸런싱 가능

---

## 4. 계층 간 설정 균형 — 50건 배치 실패 원인 분석

50건을 동시에 발사하는 부하 테스트에서 반복적으로 실패가 발생했던 원인은 **두 계층의 동시 연결 제한**이 모두 걸렸기 때문이다.

### 병목 1: Uvicorn limit-concurrency 50

50건이 동시에 도착하면 슬롯이 꽉 차서, 헬스체크나 타이밍 차이로 51번째 요청이 들어오면 `503 Service Unavailable` 반환.

→ **100으로 상향하여 해결.**

### 병목 2: Nginx keepalive 16

Nginx가 백엔드로 유지하는 동시 연결이 16개뿐이었다. 50건 요청이 들어오면 최대 16개만 동시 전달되고 나머지는 Nginx 큐에서 대기하면서 연결이 끊기거나 실패.

→ **64로 상향하여 해결.**

### 설정 간 균형 원칙

```
클라이언트 → Nginx (keepalive 64) → Uvicorn (limit-concurrency 100)
                                           ↓
                                    worker_connections 256
```

> `Nginx keepalive ≥ 예상 동시 요청 수`가 되어야 Nginx가 병목이 되지 않는다. 단, `worker_connections`(256) 범위 내에서 설정해야 한다.

각 계층의 설정이 독립적으로 합리적이어도, **하위 계층의 용량이 상위 계층보다 작으면 하위 계층이 병목이 된다.** 최종 설정:

| 계층 | 설정 | 값 |
|---|---|---|
| Nginx | keepalive | 64 |
| Uvicorn | limit-concurrency | 100 |
| Uvicorn | backlog | 256 |
| Nginx | worker_connections | 256 |

---

## 정리

| 계층 | 핵심 설정 | 주의점 |
|---|---|---|
| Uvicorn | `limit-concurrency` | 예상 최대 동시 요청 + 여유분. 너무 높으면 OOM |
| Uvicorn | `backlog` | TCP 대기 큐. 커넥션 거부 방지 |
| MySQL | `pool_size` | 동시 DB 접근 수에 맞춰 설정. 기본 적용 권장 |
| Nginx | `keepalive` | ≥ 예상 동시 요청 수. Uvicorn보다 작으면 Nginx가 병목 |
| Nginx | `worker_connections` | keepalive + 외부 연결 합산. 전체 상한 |

**핵심 원칙:** 각 계층의 설정을 독립적으로 보지 말고, 요청 흐름의 전체 경로에서 가장 좁은 지점이 어디인지 파악해야 한다. 단일 GPU에서는 throughput 향상 효과가 제한적이지만, 연결 관리와 인프라 확장성 측면에서 Nginx 프록시의 가치는 명확하다.
