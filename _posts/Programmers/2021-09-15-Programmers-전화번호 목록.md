---
layout: article
title: Programmers 전화번호 목록
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
def solution(phone_book):
    _phone_book = set(phone_book)
    for i in phone_book:
        prefix = ''
        for j in i[:-1]:
            prefix += j
            if prefix in _phone_book:
                return False
    return True
```
