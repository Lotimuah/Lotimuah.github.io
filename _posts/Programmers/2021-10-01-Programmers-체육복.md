---
layout: article
title: Programmers 체육복
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
def solution(n, lost, reserve):
    received = 0
    _lost = list(set(lost) - set(reserve))
    _reserve = list(set(reserve) - set(lost))

    for taker in _lost:
        for giver in range(taker-1, taker+2):
            if giver in _reserve:
                _reserve.remove(giver)
                received += 1
                break

    answer = n - len(_lost) + received
    return answer
```
