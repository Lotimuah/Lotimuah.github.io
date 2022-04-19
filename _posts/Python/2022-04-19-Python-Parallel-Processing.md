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

### Dask?

  Dask는 python으로 작성된 병렬 컴퓨팅을 위한 오픈 소스 라이브러리이고, **가상 데이터프레임**과 **병렬처리 작업 스케줄** 기능을 가집니다.

### 1. Dask 설치

    $ pip install dask

### 2. 간단한 example

  1에서 5까지 숫자를 1초에 하나씩 출력하는 task를 병렬화해보는 간단한 example을 해봅시다.

    from dask import delayed, compute
    from time import sleep

    %%time
    def task(i):
      print(i)
      sleep(1)
      return i

    results = []
    for i in range(1, 6):
      result = task(i)
      results.append(result)

    print(results)

    1
    2
    3
    4
    5
    [1, 2, 3, 4, 5]
    CPU times: user 4.32 ms, sys: 0 ns, total: 4.32 ms
    Wall time: 5.01 s
