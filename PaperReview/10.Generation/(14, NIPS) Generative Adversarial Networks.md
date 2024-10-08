# Generative Adversarial Networks (NIPS, 2014)

[논문 링크](https://arxiv.org/abs/1406.2661)

<p align="center">
    <img width="600" alt='fig1' src="../img/goodfellow2020generative.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝은 입력 신호에 대해 class label로 매핑하는 discrimiative model 발전에 큰 영향을 주었음
- 하지만, 많은 확률적 계산이 필요하고 딥러닝의 구조상 이점을 활용하기 어려운 deep generative models에는 큰 영향을 주지 못함
- 본 연구에서는 이러한 generative model의 어려움을 극복하기 위한 adversarial nets framework를 제안함
- Adversarial framework에서 generative model은 위조지폐를 만드는 범죄자, discriminative model은 위조 지폐를 탐지하려는 경찰과 유사함: 두 모델 사이의 경쟁은 위조품이 진품과 구별될 수 없을 때까지 양 팀이 서로의 방법을 개선하도록 유도함
- 이러한 방식으로 학습하는 딥러닝 모델을 adversarial networks라고 부름
- 해당 방식을 사용하면 generative model을 위한 근사 추론이나 Markov chain 등이 더이상 불필요함

## 접근법
- Disciminator (D)는 입력 이미지가 실제 이미지인지 생성 이미지인지 잘 분류하도록 학습되며, generator (G)는 D가 생성 이미지를 실제 이미지로 최대한 착각할 수 있는 이미지를 생성하도록 학습됨
- 즉 objective function은 two-player minimax game을 따름
- 이러한 objective function은 생성 이미지 분포와 실제 이미지 분포가 같아지는 global optimum을 보장함 (수학적으로 증명)

## 실험결과
- MNIST, CIFAR-10, Toronto Face Database 등의 여러 데이터셋에서 모델 학습
- 기존 모델들보다 뛰어나다고 할 수는 없지만 adversarial 방법의 가능성을 보여줌

## 의견
- /