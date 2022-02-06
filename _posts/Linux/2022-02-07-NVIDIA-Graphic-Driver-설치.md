---
layout: article
title: NVIDIA Graphic Driver 설치
tags: Linux
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---

## Remarks

  **OS** : Ubuntu 18.04 LTS(x86_64)
  **GPU** : NVIDIA RTX 3090
  **CUDA Toolkit version** : 11.2
  **NVIDIA Driver version** : 460.27

## 1. CUDA Toolkit install file 다운로드

  [CUDA Toolkit 11.2 version](https://developer.nvidia.com/cuda-11.2.0-download-archive)에서 자신의 setting에 맞게 파일을 다운로드 합니다.

    $ 
## 2. Nouveau Driver 제거

  Ubuntu에서 기본적으로 설치되어 있는 **Nouveau driver**와 설치하려는 **NVIDIA driver**가 충돌하는 문제를 해결하기 위해 미리 제거해줍니다. 만약 NVIDIA driver가
## 3. NVIDIA Graphic Driver 설치

## 4. 환경변수 추가

## 5. NVIDIA Driver, CUDA check

    $ nvidia-smi
