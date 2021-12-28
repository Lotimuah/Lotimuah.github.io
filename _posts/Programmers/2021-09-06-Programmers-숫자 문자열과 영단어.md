---
layout: article
title: Programmers 숫자 문자열과 영단어
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
    answer = s
    hash = {'zero':'0', 'one':'1', 'two':'2', 'three':'3', 'four':'4', 'five':'5', \
            'six':'6', 'seven':'7', 'eight':'8', 'nine':'9'}
    for i, v in hash.items():
            answer = answer.replace(i, v)
    return int(answer)
```
