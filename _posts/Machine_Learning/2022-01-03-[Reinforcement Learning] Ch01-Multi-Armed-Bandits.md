---
layout: article
title: (RL) Ch01. Multi Armed Bandits
tags: Machine_Learning
aside:
  toc: true
key: page-aside
---


### Multi-Armed Bandits(MAB)란?  
  -> slot machine with unknown rewards

### What makes a Bandit problem?  
  2가지 핵심 properties:  
    1) reward에 대해 모르는 채로 action을 선택한다.  
    2) 선택에 대해 feedback(reward)이 주어진다.  

  -> reward distribution이 stationary에 가까우면 MAB 문제가 쉬워진다.

### Finite-Armed Bandits Model

  $A_t \in \{1, 2, ... , k\}$  
  $R_t \sim_{iid} P_{A_t}\qquad\qqaud Unknown dist.$  
  $q_*(a) = E\[R_t|A_t = a\]\qquad 각 action에 대한 R_t의 분포를 모르므로 true action value를 알 수 없다.$  