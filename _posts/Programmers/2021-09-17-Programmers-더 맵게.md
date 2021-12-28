---
layout: article
title: Programmers 더 맵게
tags: Programmers
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---



```python
import heapq

def solution(scoville, K):
    cnt = 0
    heapq.heapify(scoville)
    while len(scoville) > 1 and scoville[0] < K:
        tmp = heapq.heappop(scoville)
        tmp += heapq.heappop(scoville) * 2
        heapq.heappush(scoville, tmp)
        cnt += 1

    if len(scoville) < 2 and scoville[0] < K:
        cnt = -1

    return cnt
```
