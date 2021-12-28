---
layout: article
title: Programmers 기능 개발
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

def solution(progresses, speeds):
    answer = []
    rest = deque()

    for i, v in enumerate(progresses):
        if (100 - v) % speeds[i] == 0:
            rest.append((100 - v) // speeds[i])
        else:
            rest.append(((100 - v) // speeds[i]) + 1)

    cnt = 0

    while rest:
        tmp = rest.popleft()
        cnt += 1

        if not rest:
            answer.append(cnt)
            return answer

        while True:
            if not rest:
                answer.append(cnt)
                return answer
            if tmp < rest[0]:
                answer.append(cnt)
                cnt = 0
                break
            rest.popleft()
            cnt += 1

    return answer
```
