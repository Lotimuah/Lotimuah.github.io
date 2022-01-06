---
layout: article
title: DuelingDQN 논문 정리
tags: Paper_Review
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
use_math: true
---

  \* 이 포스트는 DeepMind의
  &nbsp;&nbsp;[Dueling Network Architectures for Deep Reinforcement Learning](https://arxiv.org/pdf/1511.06581.pdf) 논문을 정리한 글입니다.

  ----------------------------------------------------------------------

**분류** : Model-free/Deep Q-learning/DuelingDQN

## Domain Knowledge

  이전 포스팅 [DoubleDQN 논문 정리](https://loteeyoon.github.io/2022/01/05/DoubleDQN-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)에 이어 이번에 소개할 DuelingDQN은 DQN의 architecture를 dueling 형태로 만들어 다시 Atari domain에서 SOTA를 달성했습니다. DQN 이후로 RL에서는 deep representation을 사용하는 데 많은 성공을 거두었으나 application의 대부분은 CNN, LSTM, Auto-Encoder와 같은 기존 architecture를 사용하는 것에 그쳤습니다. DoubleDQN이 DQN의 target value 계산 방식을 개선했다면, 이 논문에서는 Neural network가 근사하려고 하는 $Q(s, a)$를 state의 value와 action의 advantage로 나누어질 수 있다는 아이디어에 기반해 Dueling network라는 두 개의 개별 estimator를 두고 value function과 advantage function을 구해 이들을 통합하여 $Q(s, a)$를 구합니다. 두 function estimator가 동시에 Q를 estimate한다는 점에서 dueling이라는 이름 붙일 수 있으며 이러한 factoring은 기본 RL 알고리즘을 변경하지 않고 action 전반에 걸쳐 학습을 일반화 가능하게 합니다.

## Methods

  
