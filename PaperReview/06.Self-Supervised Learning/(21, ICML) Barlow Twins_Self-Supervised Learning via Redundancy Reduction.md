# Barlow Twins: Self-Supervised Learning via Redundancy Reduction (ICML, 2021)

[논문링크](http://proceedings.mlr.press/v139/zbontar21a.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/zbontar2021barlow.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근 self-supervised 연구의 큰 공통점은 augmented된 두 views의 유사도를 최대화하는 contrastive 기반이라는 것
- 이러한 contrastive 기반의 방법들은 constant representation과 같은 trivial solutions이 존재하므로 이를 방지하기 위한 여러 방법들이 제안되어 왔음
> - SimCLR: positive samples와 함께 negative samples의 개념을 사용함
> - MoCo: asymmetric한 업데이트 전략을 사용
> - DeepCluster, SwaV: 클러스터링 기반의 방법
> - BYOL, SimSiam: asymmetric한 아키텍처 및 업데이트 전략을 사용
- 본 논문에서는 redundancy-reduction 원칙을 적용한 새로운 self-supervised method인 Barlow Twins(BT)를 제안함
- BT는 augmented된 두 views의 embeddings 간의 cross-correlation matrix가 identity matrix와 가까워지도록 학습함
- BT는 매우 직관적이며 구현이 쉽고, asymmetric 아키텍처나 큰 배치 사이즈 등을 필요로 하지 않음 

## 접근법
- BT도 다른 contrastive methods와 마찬가지로 random augmentation function에 의해 distorted된 두 views를 입력으로 사용함
- 두 views는 가중치를 공유하는 네트워크와 projector를 거쳐 embedding으로 projection됨
- 두 views에 대해 cross-correlation matrix를 계산함 (ex. batch size=8, output dimension=256일 때 cross-correlation matrix의 dimension은 256x256)
- BT의 loss function은 cross-correlation matrix의 diagonal values는 1에 가까워지도록(invariance term), 그 외의 values들은 0에 가까워지도록(redundancy reduction term) 함
- Invariance term은 두 views의 표현이 유사해지도록 하는 역할(기존의 contrastive loss)을 하며, redundancy reduction term은 embedding의 서로 다른 구성요소들의 상관관계를 제거하여 sample에 대한 불필요하고 중복되는 정보를 제거함
- 이러한 loss term은 Info-NCE loss와 비슷해보일 수 있지만 BT loss는 negative samples를 사용하지 않으므로 큰 배치사이즈를 요구하지 않으며 high dimensinal outputs에서 잘 기능한다는 점에서 다름

## 실험결과
- linear evaluation, semi-supervised setup, detection으로의 transfer setup에서 SOTA 혹은 SOTA와 근접한 성능 달성
- 배치사이즈에 대해서는 민감하지 않지만 augmentation methods에 대해서는 민감함
- Projection layer의 hidden dimension이 높을 수록 성능이 증가함
> - 더 많은 (유용한) 정보를 담을 수 있기 때문
> - BT는 redundancy reduction term을 통해 embedding vector에서 중복되는 정보가 없도록 만들어주기 때문에 각 column이 sample에 대한 고유한 정보를 담을 수 있도록 함
- Asymmetric한 구조를 만들어도, collapse problem이 일어나진 않음

## 의견
- /