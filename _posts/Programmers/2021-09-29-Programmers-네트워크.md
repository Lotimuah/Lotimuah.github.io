---
layout: article
title: Programmers 네트워크
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

## DFS

```python
def solution(n, computers):
    answer = 0
    visited = n*[0]

    def dfs(start):
        for i in range(n):
            if i != start and visited[i] == 0 and computers[start][i] == 1:
                visited[i] = 1
                dfs(i)

    for i in range(n):
        if visited[i] == 1:
            continue
        answer += 1
        dfs(i)
    return answer
```

## BFS

```python
from collections import deque

def solution(n, computers):
    answer = 0
    visited = n*[0]

    def bfs(start):
        queue = deque()
        queue.append(start)
        while len(queue) != 0:
            start = queue.popleft()
            visited[start] = 1
            for i in range(n):
                if i != start and visited[i] == 0 and computers[start][i] == 1:
                    queue.append(i)

    for i in range(n):
        if visited[i] == 1:
            continue
        answer += 1
        bfs(i)

    return answer
```
