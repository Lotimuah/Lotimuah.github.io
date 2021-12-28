---
layout: article
title: Programmers 프린트
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

def solution(priorities, location):
    task = deque([chr(i) for i in range(65, 65+len(priorities))])
    priorities = deque(priorities)
    bucket = []
    location = task[location]

    while priorities:
        tmp = priorities.popleft()
        if not priorities:
            bucket.append(task.popleft())
            break

        if tmp > max(priorities) or tmp == max(priorities):
            bucket.append(task.popleft())
        else:
            priorities.append(tmp)
            task.append(task.popleft())

    return bucket.index(location) + 1
```
