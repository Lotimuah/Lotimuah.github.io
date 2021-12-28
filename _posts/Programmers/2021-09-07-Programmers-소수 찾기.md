---
layout: article
title: Programmers 소수 찾기
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
from itertools import permutations

def solution(numbers):
    answer = 0

    def is_prime(x):
        for i in range(2, x-1):
            if x % i == 0:
                break
        else:
            return x

    numbers = list(numbers)
    per = []
    for i in range(1, len(numbers)+1):
        per += permutations(numbers, i)

    numbers = list(set([int(''.join(i)) for i in per]))
    answer = list(filter(is_prime, numbers))
    answer = len([i for i in answer if i > 1])
    return answer
```
