---
layout: article
title: Parallel Processing
tags: Python
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---

  \* 이 포스트는 python에서 process를 병렬화할 수 있는 방법에 대해 간단하게 정리했습니다.

  ----------------------------------------------------------------------

  많은 데이터를 처리하거나 시간이 오래 걸리는 머신러닝 학습을 할 경우 병렬처리가 효율적입니다. 이 경우 Dask라는 라이브러리를 사용하면 병렬처리가 가능해집니다.

  **Dask**

  Dask는 python으로 작성된 병렬 컴퓨팅을 위한 오픈 소스 라이브러리입니다.
