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

  이전 포스팅 [DoubleDQN 논문 정리](https://loteeyoon.github.io/2022/01/05/DoubleDQN-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)와 [PER 논문 정리](https://loteeyoon.github.io/2022/01/06/PER-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)에 이어 이번에 소개할 DuelingDQN은 DQN의 architecture를 dueling 형태로 만들어 다시 Atari domain에서 SOTA를 달성했습니다. DQN 이후로 RL에서는 deep representation을 사용하는 데 많은 성공을 거두었으나 application의 대부분은 CNN, LSTM, Auto-Encoder와 같은 기존 architecture를 사용하는 것에 그쳤습니다. DoubleDQN이 DQN의 target value 계산 방식을 개선했고 PER이 좀 더 중요도가 높은 sample에 집중하도록 sampling 방식을 개선했다면, DuelingDQN은 Neural network가 근사하려고 하는 $Q(s, a)$가 state의 value $V(s)$와 action의 value $A(s, a)$로 나누어질 수 있다는 아이디어에 기반해 Dueling network라는 두 개의 개별 estimator를 두고 **Value function**과 **Advantage function**을 구해서 이들을 통합하는 방식으로 $Q(s, a)$를 계산합니다. 두 function estimator가 동시에 Q를 estimate한다는 점에서 dueling이라는 이름 붙일 수 있으며 이러한 factoring은 기본 RL 알고리즘을 변경하지 않고 action 전반에 걸쳐 학습을 일반화 가능하게 합니다.

## Dueling Network

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/dueling_architect.png?raw=true"></p>  

  Fig 1.을 보면 기존의 Q-network와 dueling Q-network는 거의 차이가 없지만 마지막 부분이 조금 다르다는 것을 알 수 있습니다. input으로 raw screen을 받아들여 feature를 추출하고 Q-function을 근사하는 부분은 동일하지만 끝 부분에서 2개의 stream으로 분리되었다가 최종적으로 합쳐져 Q-function estimation을 합니다. 위쪽 stream은 state-value를 계산하는 **Value stream**이고, 아랫쪽 stream은 action의 value를 계산하는 **Advantage stream**입니다. 이렇게 간접적으로 $Q(s, a)$를 구하는 이유에 대해서 논문에서는 장애물을 회피하는 Atari enduro 게임의 simulation을 통해 설명합니다.


<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/Dueling_enduro_game.png?raw=true"></p>  

  Fig 2.는 장애물이 없는 경우(Top)와 있는 경우(Bottom)에서 agent가 expected return을 높이기 위해 어디에 focus를 둬야할 지 value(left)와 advantage(right) 각각의 관점에서 보여줍니다. 자세히 살펴보면 value 관점에선 결국 expected return과 관련있는 목적지에 집중하게 되므로 지평선 근처의 차선 부근에 붉게 표시가 됩니다. advantage 관점에선 장애물이 없는 경우는 당장 action을 취하지 않아도 reward에 영향이 없기 때문에 별다른 focusing이 없습니다. 그러나 장애물이 있는 경우에는 붉게 표시가 나타나며 focusing을 하게 됩니다.

  이와 같이 각각의 stream에서 $Q(s, a)$를 value $V(s)$와 advantage $A(s, a)$로 나누어 계산할 수 있게 되면서 효과적으로 attention을 할 수 있습니다. 따라서 추정하고자 하는 $Q(s, a)$를 다음과 같은 식으로 표현할 수 있습니다.

<br/>

$$
\begin{aligned}
Q(s, a; \theta, \alpha, \beta) = V(s; \theta, \beta) + A(s, a; \theta, \alpha)\\
\end{aligned}
$$

<br/>

  여기서 Advatage function $A(s, a)$는 이 논문에서 제안하는 새로운 함수가 아니라 기존에 사용하던 함수입니다. RL에서는 종종 어떤 action이 '절대적으로 좋은가' 보다는 '다른 것보다 얼마나 좋은가'에 초점을 맞춥니다. 이러한 것을 Advatage function을 통해 나타낼 수 있는데 다음과 같은 식으로 표현할 수 있습니다.

<br/>

$$
\begin{aligned}
A^{\pi}(s, a) = Q^{\pi}(s, a) - V^{\pi}(s)
\end{aligned}
$$

<br/>

  그래서 이 Advantage function을 이용해 표현한 $Q(s, a)$를 학습에 적용할 수 있습니다. 그러나 여기서 식을 그대로 사용하면 **identifibility** 문제가 발생합니다. 우리가 실제 학습에서 $Q(s, a)$가 주어졌을 때, $Q(s, a)$가 unique value가 아니기 때문에 $V(s; \theta, \beta)$ 와 $A(s, a; \theta, \alpha)$를 구분할 수 없게 된다는 것입니다.

  이 문제를 해결하기 위해 논문에서는 prior information을 사용합니다. 우리는 state-value가 action-value의 expectation 값이라는 것을 알고 있습니다.

<br/>

$$
\begin{aligned}
V^{\pi}(s) = \mathbb{E}_{a \sim \pi(s)}[Q^{\pi}(s, a)]
\end{aligned}
$$

<br/>

  이때, 위에서 정의한 advantage 식의 양 변에 expectation을 취한다면 다음이 성립합니다.

<br/>

$$
\begin{aligned}
\mathbb{E}_{a \sim \pi(s)}[Q^{\pi}(s, a)] &= \mathbb{E}_{a \sim \pi(s)}[V^{\pi}(s) + A^{\pi}(s, a)]\\
V^{\pi}(s) &= V^{\pi}(s) + \mathbb{E}_{a \sim \pi(s)}[A^{\pi}(s, a)]\\
\end{aligned}
$$

<br/>

  두 번째 식에서 expectation이 action에 대한 것이므로 state-value $V^{\pi}(s)$는 expectation에서 빠져나오고 결과적으로 다음이 성립합니다.

<br/>

$$
\begin{aligned}
\therefore \mathbb{E}_{a \sim \pi(s)}[A^{\pi}(s, a)] &= 0
\end{aligned}
$$

<br/>

또 하나 알 수 있는 사실은 optimal action $a^*$일 때, 다음이 성립한다는 것입니다.

<br/>

$$
\begin{aligned}
Q(s, a^*) = max_aQ(s, a) = V(s)\\
\end{aligned}
$$

$$
\begin{aligned}
\therefore A(s, a^*) = 0\\
\end{aligned}
$$

<br/>

  이는 deterministic policy에 대해 항상 max action-value를 선택한다면 expectation을 취해도 max value일 것이므로 그 값이 $V(s)$값과 동일하기 때문입니다. 결국 이러한 경우 advantage는 0이 됩니다.

  논문에서는 이러한 prior information을 활용해 새로운 advantage 식을 제시합니다.

<br/>

$$
\begin{aligned}
Q(s, a; \theta, \alpha, \beta) = V(s; \theta, \beta) + \biggl( A(s, a; \theta, \alpha) - max_{a' \in |\mathcal{A}|}A(s, a'; \theta, \alpha) \biggr)\\
\end{aligned}
$$

<br/>

  위 식에서 우변의 두 번째 term을 보면 parameter $\alpha$에 의해 근사되는 $A(s, a)$는 완전히 $A(s, a)$의 정의를 따르진 않을 것이지만 $max_{a' \in |\mathcal{A}|}A(s, a'; \theta, \alpha)$라는 identifier를 통해 괄호안의 수식이 original advantage $A(s, a)$의 property를 따를 수 있게끔 해줍니다. 결국 optimal action일 경우 위의 prior information을 통해 괄호 term은 $A(s, a^*) - maxA_{a'}(s, a') = 0$이 될 것이고 state-value $V(s)$ 값을 구분할 수 있게 됩니다.

  논문에서는 $\max$ 가 아닌 평균을 이용하는 방법도 제안했습니다.

<br/>

$$
\begin{aligned}
Q(s, a; \theta, \alpha, \beta) = V(s; \theta, \beta) + \biggl( A(s, a; \theta, \alpha) - \frac{1}{|\mathcal{A}|} \sum_{a'}A(s, a'; \theta, \alpha) \biggr)\\
\end{aligned}
$$

<br/>

  위에서 advantage $A^{\pi}$의 expectation은 0이라는 것을 이용하면 첫 번째 방법과 동일합니다. 다만, 평균을 빼줌으로써 실제적인 $V$ 와 $A$의 property와는 조금 멀어지게 되지만 action간의 ranking은 여전히 보존하므로 학습에 있어서 안정성을 준다고 논문에서는 서술하고 있습니다.
