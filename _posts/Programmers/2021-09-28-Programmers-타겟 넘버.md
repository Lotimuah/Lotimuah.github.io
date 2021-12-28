---
layout: article
title: Programmers 타겟 넘버
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
from itertools import product

def solution(numbers, target):
    answer = 0
    sign = ['+', '-']
    sign_product = list(product(sign, repeat=len(numbers)))
    sign_product = [[*i] for i in sign_product]
    
    for i in sign_product:
        tmp = 0
        for j in range(len(i)):
            if i[j] == '+':
                tmp += numbers[j]
            else:
                tmp -= numbers[j]
        if tmp == target:
            answer += 1
    return answer
```
