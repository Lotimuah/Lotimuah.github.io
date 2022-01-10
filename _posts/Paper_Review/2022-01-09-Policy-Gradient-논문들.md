---
layout: article
title: Policy Gradient 논문들
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

  \* 이 포스팅은 Policy Gradient 계열의 주요 논문들에 대한 핵심 내용을 아주 간단하게 정리한 글입니다.

  --------------------------------------------------------------

  **분류** : Model-free/Policy Gradient  


  RL 알고리즘은 크게 Value-based 알고리즘과 Policy-based 알고리즘으로 나눌 수 있습니다. Value-based 알고리즘은 각 state마다 action에 대해 가치를 판단하여 간접적으로 implicit policy를 구하는 것입니다. 그러나 policy-based 알고리즘은 직접적으로 policy를 optimize하는 방식의 알고리즘입니다. policy-based 알고리즘은 value-based에 비해 수렴이 더 잘 되며, action space가 많거나(high-dimension) 연속적인(continuous) 경우에 적합합니다. 또한 stochastic한 policy를 학습하기 때문에 deterministic한 policy가 optimal이 될 수 없는 상황에 유리합니다. 이번 포스팅에서는 이러한 policy-based 알고리즘의 대표적인 논문들을 정리할 것입니다.


## 1. REINFORCE


## 2. Actor-Critic

## 3. Advantage Actor-Critic (A2C)

## 4. Asyncronous Advantage Actor-Critic (A3C)

## 5. Trust Region Policy Gradient (TRPO)

## 6. Proximal Policy Optimization (PPO)

## 7. Deep Deterministic Policy Gradient (DDPG)

## 8. Soft Actor-Critic (SAC)
