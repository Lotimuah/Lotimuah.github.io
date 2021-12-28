---
layout: article
title: Programmers 완주하지 못한 선수
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
import collections

def solution(participant, completion):
    completion_ = collections.defaultdict(int)
    participant_ = collections.defaultdict(int)
    answer = ''
    for i in completion:
        completion_[i] += 1

    for j in participant:
        participant_[j] += 1

    for k in participant:
        if completion_[k] != participant_[k]:
            answer = k
    return answer
```
