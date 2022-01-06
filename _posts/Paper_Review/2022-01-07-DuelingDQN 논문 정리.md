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

  이전 포스팅 [DoubleDQN 논문 정리](https://loteeyoon.github.io/2022/01/05/DoubleDQN-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)과 [PER 논문 정리](https://loteeyoon.github.io/2022/01/06/PER-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC.html)에 이어 이번에 소개할 DuelingDQN은 DQN의 architecture를 dueling 형태로 만들어 다시 Atari domain에서 SOTA를 달성했습니다. DQN 이후로 RL에서는 deep representation을 사용하는 데 많은 성공을 거두었으나 application의 대부분은 CNN, LSTM, Auto-Encoder와 같은 기존 architecture를 사용하는 것에 그쳤습니다. DoubleDQN이 DQN의 target value 계산 방식을 개선했고 PER이 좀 더 중요도가 높은 sample에 집중하도록 sampling 방식을 개선했다면, DuelingDQN은 Neural network가 근사하려고 하는 $Q(s, a)$가 state의 value와 action의 value로 나누어질 수 있다는 아이디어에 기반해 Dueling network라는 두 개의 개별 estimator를 두고 **Value function**과 **Advantage function**을 구해서 이들을 통합하는 방식으로 $Q(s, a)$를 계산합니다. 두 function estimator가 동시에 Q를 estimate한다는 점에서 dueling이라는 이름 붙일 수 있으며 이러한 factoring은 기본 RL 알고리즘을 변경하지 않고 action 전반에 걸쳐 학습을 일반화 가능하게 합니다.

## Dueling Network

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/dueling_architect.png?raw=true"></p>  
<br/>

  Fig 1.을 보면 기존의 Q-network와 dueling Q-network는 거의 차이가 없지만 마지막 부분이 조금 다르다는 것을 알 수 있습니다. input으로 raw screen을 받아들여 feature를 추출하고 Q-function을 근사하는 부분은 동일하지만 끝 부분에서 2개의 stream으로 분리되었다가 최종적으로 합쳐져 Q-function estimation을 합니다. 위쪽 stream은 state-value를 계산하는 **Value stream**이고, 아랫쪽 stream은 action의 value를 계산하는 **Advantage stream**입니다. 이렇게 간접적으로 $Q(s, a)$를 구하는 이유에 대해서 논문에서는 장애물을 회피하는 Atari enduro 게임의 simulation을 통해 설명합니다.


<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/Dueling_enduro_game.png?raw=true"></p>  

  장애물이 있는 경우(Top)와 없는 경우(Bottom)에서 agent가 e
