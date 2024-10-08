# Weight Agnostic Neural Networks (NIPS, 2019)

[논문링크](https://proceedings.neurips.cc/paper/2019/hash/e98741479a7b998f88b8f8c9f0b6b6f1-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/gaier2019weight.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 randomly-initialized된 parameters를 가지고도 주어진 task를 해결할 수 있는 neural network를 설계함
> - 강한 inductive bias를 주입하여 자연스럽게 주어진 task를 잘 수행하도록 함
> - Weight agnostic neural networks (WANN)
- 고정된 network의 weights를 최적화하는 것이 아닌, 다양한 single weight 하에서도 잘 동작하도록 아키텍처를 최적화

## 접근법
- 본 논문에서 최적의 아키텍처를 찾는 과정(WANN)은 NAS와는 다름
> - NAS는 인간에 의해 디자인된 모델 아키텍처보다 성능이 뛰어난 아키텍처를 찾는 과정
> - NAS로 찾은 네트워크도 결국 학습이 필요함
> - 본 논문의 접근법은 solution 자체를 포함하고 있는 최적의 아키텍처를 찾는 것 (학습 없이)
- Optimized된 네트워크의 성능으로 네트워크를 평가하는 것이 아닌 random하게 sampling된 parameters를 사용했을 때의 성능을 평가
> - Weight training을 weight sampling으로 변경
- 이때 모든 개별 파라미터에 대해 weight sampling을 수행하는 것은 infeasible하므로, 단일 weight을 sampling하고 모든 네트워크의 parameter가 해당 값을 가지게 함
- Search의 각 단계는 다음과 같음:
> - (1) 아주 간단한 네트워크 topologies 몇 개를 정의하고,
> - (2) 여러 sampled weight으로 각각의 topology를 평가하고,
> - (3) 복잡도와 성능에 대해 각 네트워크를 정렬하고, 
> - (4) 좋았던 네트워크 topologies를 일부 결합하여 새로운 topologies를 생성해나감
- 새로운 topologies를 생성할 때는 (i) node 추가, (ii) node간 연결 추가, (iii) activation function 변경의 세 가지 operators 중 일부를 적용함
 
## 실험결과
- CartPoleSwingUp, BipedalWalker-v2, CarRacing-v0 tasks에서 평가함
- WANN으로 찾은 네트워크는 random하게 sampling된 shared weight를 사용하더라도 준수한 성능을 보여줌
> - WANN으로 찾은 네트워크는 SOTA baseline의 topology와 달리 학습이 필요 없을 뿐만 아니라 훨씬 적은 connections로 구성됨
- Classification task에서도 좋은 성능을 기록함

## 의견
- / 