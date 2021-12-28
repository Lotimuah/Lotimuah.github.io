---
layout: article
title: Programmers 여행 경로
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
from collections import defaultdict

def solution(tickets):
    answer = []
    graph = defaultdict(list)

    for i, j in tickets:
        graph[i].append(j)

    for i in graph.keys():
        graph[i].sort(key=str)

    stack = ['ICN']
    path = []

    def dfs():
        while stack:
            tmp = stack[-1]
            if graph[tmp] != []:
                stack.append(graph[tmp].pop(0))
            else:
                path.append(stack.pop())

        return path[::-1]
    
    answer = dfs()
    return answer
```
