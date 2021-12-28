---
layout: article
title: Programmers 모의고사
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
def solution(answers):
    answer = []
    p_1 = [1, 2, 3, 4, 5]                     
    p_2 = [2, 1, 2, 3, 2, 4, 2, 5]            
    p_3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]     
    point = [0, 0, 0]
    for i in range(len(answers)):
        if answers[i] == p_1[i % 5]:
            point[0] += 1
        if answers[i] == p_2[i % 8]:
            point[1] += 1
        if answers[i] == p_3[i % 10]:
            point[2] += 1
    print(point)
    m = max(point)
    print(m)
    temp = [i for i, v in enumerate(point) if m == v]
    print(temp)
    for i in temp:
        answer.append(i+1)
    answer.sort()
    return answer
```
