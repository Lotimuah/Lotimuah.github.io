---
layout: article
title: DoubleDQN 논문 정리
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

  \* 이 포스트는 DeepMind의 [Deep Reinforcement Learning with Double Q-learning](https://arxiv.org/pdf/1509.06461.pdf) 논문에 대한 내용을 정리한 글입니다.

  ----------------------------------------------------------------------

**분류** : Model-free/Deep Q-learning/DoubleDQN  

## Problems

  이전 포스팅[[DQN 논문 정리](https://loteeyoon.github.io/2022/01/04/DQN-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)]에서 RL에 Neural Network를 성공적으로 적용시킨 DQN에 대해 알아보았습니다.

## Methods
