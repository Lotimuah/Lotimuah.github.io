---
layout: article
title: Programmers 단어 변환
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
import copy

def solution(begin, target, words):
    answer = 0

    if target not in words:
        return answer

    start_words = [begin]
    reachable_words = []

    while True:
        for start_word in start_words:
            reachable_words.clear()
            for word in copy.deepcopy(words):
                diff_cnt = 0
                for i in range(len(begin)):
                    if start_word[i] != word[i]:
                        diff_cnt += 1
                if diff_cnt == 1:
                    reachable_words.append(word)
                    words.remove(word)

        answer += 1

        if target in reachable_words:
            return answer
        else:
            start_words = copy.deepcopy(reachable_words)
```
