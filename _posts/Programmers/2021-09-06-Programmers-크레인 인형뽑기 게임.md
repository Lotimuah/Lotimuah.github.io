---
layout: article
title: Programmers 크레인 인형뽑기 게임
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
def solution(board, moves):
    box = []
    count = 0

    for i in moves:
        for j in board:
            if j[i-1] == 0:
                continue
            else:
                box.append(j[i-1])
                j[i-1] = 0
                if len(box) > 1:
                    if box[-1] == box[-2]:
                        box.pop()
                        box.pop()
                        count += 2
                break

    return count
```
