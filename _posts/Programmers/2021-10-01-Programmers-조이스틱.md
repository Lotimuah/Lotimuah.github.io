---
layout: article
title: Programmers 조이스틱
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
def solution(name):
    answer = 0
    alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    move = []
    default_move = len(name) - 1

    for idx, char in enumerate(name):
        if alphabet.index(char) >= 13:
            move.append(26 - alphabet.index(char))
        elif alphabet.index(char) < 13:
            move.append(alphabet.index(char))

        next = idx + 1
        while next < len(name) and name[next] == 'A':
            next += 1

        default_move = min(default_move, 2*idx + len(name) - next)
    answer += default_move + sum(move)
    return answer
```
