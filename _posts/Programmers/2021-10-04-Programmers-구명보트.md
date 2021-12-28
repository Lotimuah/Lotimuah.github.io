---
layout: article
title: Programmers 구명보트
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
from collections import deque

def solution(people, limit):
    answer = 0
    people = deque(sorted(people))

    while people:
        if len(people) < 2:
            people.popleft()
            answer += 1
        else:
            if people[0] + people[-1] <= limit:
                people.popleft()
                people.pop()
                answer += 1
            else:
                people.pop()
                answer += 1

    return answer

hello!
```
