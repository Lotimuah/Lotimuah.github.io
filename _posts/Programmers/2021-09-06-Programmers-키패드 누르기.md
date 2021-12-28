---
layout: article
title: Programmers 키패드 누르기
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
def solution(numbers, hand):
    answer = ''
    key_pad = [[1, 2, 3], [4, 5, 6], [7, 8, 9], ['*', 0, '#']]
    r_pos = [3, 2]
    l_pos = [3, 0]

    def find_index(i):
        for row in range(4):
            for column in range(3):
                if key_pad[row][column] == i:
                    return [row, column]

    for i in numbers:
        key_pos = find_index(i)
        if key_pos[1] == 0:
            answer += 'L'
            l_pos = key_pos
        elif key_pos[1] == 1:
            d_r = abs(key_pos[0] - r_pos[0]) + abs(key_pos[1] - r_pos[1])
            d_l = abs(key_pos[0] - l_pos[0]) + abs(key_pos[1] - l_pos[1])
            if d_r > d_l:
                answer += 'L'
                l_pos = key_pos
            elif d_r < d_l:
                answer += 'R'
                r_pos = key_pos
            else:
                if hand == 'right':
                    answer += 'R'
                    r_pos = key_pos
                else:
                    answer += 'L'
                    l_pos = key_pos
        elif key_pos[1] == 2:
            answer += 'R'
            r_pos = key_pos

    return answer
```
