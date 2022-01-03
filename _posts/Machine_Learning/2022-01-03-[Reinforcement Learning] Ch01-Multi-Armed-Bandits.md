---
layout: article
title: (RL) Ch01. Multi Armed Bandits
tags: Machine_Learning
aside:
  toc: true
key: page-aside
---


* Multi-Armed Bandits(MAB)란?
  -> slot machine with unknown rewards

* what makes a Bandit problem?
  2가지 핵심 properties:
    1) reward에 대해 모르는 채로 action을 선택한다.
    2) 선택에 대해 feedback(reward)이 주어진다.

    -> reward distribution이 stationary에 가까우면 MAB 문제가 쉬워진다.

* Finite-Armed Bandits Model

  $A_t \in \{ 1, 2, ... , k \}$
