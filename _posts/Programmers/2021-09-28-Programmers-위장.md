---
layout: article
title: Programmers 위장
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
from collections import defaultdict

def solution(clothes):
    answer = 1
    closet = defaultdict(list)

    for i in clothes:
        closet[i[1]].append(i[0])

    for j in closet.keys():
        answer *= len(closet[j]) + 1

    return answer - 1
```
