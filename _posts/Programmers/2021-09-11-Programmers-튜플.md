---
layout: article
title: Programmers 튜플
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
def solution(s):
    answer = []
    s = s[2:-2].split("},{")
    for i in range(len(s)):
        s[i] = s[i].split(",")

    s.sort(key=len)

    for i in s:
        for j in i:
            if int(j) not in answer:
                answer.append(int(j))
    return answer
```
