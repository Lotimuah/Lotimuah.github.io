---
layout: article
title: Programmers H-Index
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
def solution(citations):
    answer = 0
    h_log = []
    for h in range(10000):
        if len([i for i in citations if i >= h]) >= h:
            h_log.append(h)
    answer = max(h_log)
    return answer
```
