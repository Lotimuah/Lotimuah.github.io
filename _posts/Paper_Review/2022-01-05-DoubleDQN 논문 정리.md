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

  \* 이 포스트는 DeepMind의 [Deep Reinforcement Learning with Double Q-learning](https://arxiv.org/pdf/1509.06461.pdf) 논문을 정리한 글입니다.

  ----------------------------------------------------------------------

**분류** : Model-free/Deep Q-learning/DoubleDQN  

## Domain Knowledge

  이전 포스팅[[DQN 논문 정리](https://loteeyoon.github.io/2022/01/04/DQN-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)]에서 RL에 Neural Network를 성공적으로 적용시킨 DQN에 대해 알아봤습니다. DQN이 성공적으로 Atari 문제를 푸는 데 성공했지만, Q-learning 자체가 가진 overestimation 문제까지는 풀지 못했습니다. Q-learning은 가장 널리 사용되는 RL 알고리즘 중 하나지만, 이전부터 action-value를 overestimate 한다고 알려져 왔습니다. Thrun과 Schwartz가 1993년 발표한 [Issues in Using Function Approximation for Reinforcement Learning](https://www.ri.cmu.edu/pub_files/pub1/thrun_sebastian_1993_1/thrun_sebastian_1993_1.pdf) 논문에서는 approximation 과정에서 생기는 noise가 max operation으로 인해 action-value를 overestimate하고 이것이 optimal policy 학습을 방해한다고 주장했습니다. 이 논문에서는 위 논문의 주장을 확장해서 max operation 외에도 다른 여러 요인들이 overestimate 문제를 일으킨다는 것을 실험을 통해 규명하고 이를 해결할 수 있는 방법을 제안합니다.


## Problems

  DQN에서 우리는 Q-learning의 target을 다음과 같이 사용하였습니다.

$$
\begin{aligned}
Y^{DQN}_{t} \equiv R_{t+1} + \gamma \max_a Q(s_{t+1}, a; \; \theta^-_t)\\
\end{aligned}
$$

  여기서 max operation로 인해 function approximator가 따라가야할 target이 실제값보다 크게 추정됩니다. 이는 Neural network를 function approximator로 사용하지 않는 기본 Q-learning에서도 나타났던 문제입니다. 이에 대한 원인을 저자는 이렇게 설명하고 있습니다.

<br/>

>Overestimation is due to the fact that the expectation of a maximum is greater than or equal to the maximum of an expectation, often with a strict inequality (see, e.g., Theorem 1 in https://arxiv.org/abs/1302.7175).

<br/>

  Q-learning에서는 next state $s'$의 expected value의 최댓값 $max_{a'}\mathbb{E}[ Q(s', a') ]$을 구하는 것이 목적입니다. next state에서의 action 중 어느 action의 기댓값이 가장 큰지 알고 싶은데 Q-learning에서는 이를 위해 단지 $\max_{a'}Q(s', a')$으로 estimate합니다. 그러나 이때 Non-negative bias가 존재하게 되고 $\max_{a'}Q(s', a')$는 평균적으로 어떠한 $a'$의 action-value의 기댓값 보다도 높은 값을 가지게 됩니다.

$$
\begin{aligned}
\mathbb{E}[\max_{a'} Q(s', a')] \geq \max_{a'} \mathbb{E}[Q(s', a')]
\end{aligned}
$$

<br/>

  논문에서는 위의 내용을 Theorem 1.을 통해 보여주고 있습니다.  


<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/DoubleDQN_Theorem.png?raw=true"></p>

  Theorem 1.을 간단하게 설명하면 max operation을 사용하게 되면 optimal value보다 항상 최소 $\sqrt{\frac{C}{m-1}}$만큼의 bias를 가진다는 것입니다. 논문에서는 이를 **single estimator**의 문제로 규정하고 이에 대한 대안으로 **double estimator**를 제안했습니다.


## Methods

  target의 overestimate 문제를 해결하기 위해 논문에서는 **double estimator** 방식을 사용했는데, 이는 target을 계산하는 과정을 <U>next state의 action을 선택하는 과정</U>(selection)과 <U>해당 action의 값을 추정하는 과정</U>(evaluation)으로 분리하는 것입니다. 그래서 DoubleDQN의 target을 논문에서는 다음과 같이 사용합니다.


$$
\begin{aligned}
Y^{DoubleQ}_{t} \equiv R_{t+1} + \gamma Q(s_{t+1}, argmax_a Q(s_{t+1}, a; \; \theta_t); \; \theta^-_t)\\
\end{aligned}
$$

<br/>

$argmax_aQ(s_{t+1}, a; \; \theta_t)$에서 greedy action의 selection은 learning network를 통해 이루어지고, 이에 대한 가치를 target network에서 estimate합니다.











<!-- <p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/DoubleDQN_Figure_1.png?raw=true"></p> -->
