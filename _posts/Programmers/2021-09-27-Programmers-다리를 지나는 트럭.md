---
layout: article
title: Programmers 다리를 지나는 트럭
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
def solution(bridge_length, weight, truck_weights):
    bridge = [0] * bridge_length
    time = 0

    while bridge:
        time += 1
        bridge.pop(0)
        if truck_weights:
            if sum(bridge) + truck_weights[0] <= weight:
                bridge.append(truck_weights.pop(0))
            else:
                bridge.append(0)

    return time
```
