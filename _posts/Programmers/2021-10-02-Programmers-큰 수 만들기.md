---
layout: article
title: Programmers 큰 수 만들기
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
def solution(number, k):
    answer = []

    for i in number:
        if not answer:
            answer.append(i)
            continue
        if k > 0:
            while answer[-1] < i:
                answer.pop()
                k -= 1
                if not answer or k <= 0:
                    break
        answer.append(i)

    if k > 0:
        answer = answer[:-k]

    answer = ''.join(answer)
    return answer
```
