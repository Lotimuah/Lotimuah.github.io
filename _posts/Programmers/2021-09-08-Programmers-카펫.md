---
layout: article
title: Programmers 카펫
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
from math import *

def solution(brown, yellow):
    answer = []
    divisor = []
    for i in range(1, int(sqrt(yellow))+1):
        if yellow % i == 0:
            divisor.append([yellow//i, i])

    for j in divisor:
        if (j[0]+2)*(j[1]+2) - yellow == brown:
            answer = [j[0]+2, j[1]+2]
    return answer
```
