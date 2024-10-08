# Unsupervised Learning of Visual Features by Contrasting Cluster Assignments (NIPS, 2020)

[논문링크](https://proceedings.neurips.cc/paper/2020/hash/70feb62b69f16e0238f741fab228fec2-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/caron2020unsupervised.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Unsupervised visual representation learning (Self-supervised Learning)으ㄴ manual annotations 없이 좋은 피처를 학습하는 것을 목표로 하며 최근 supervised 방법과의 성능 격차를 매우 좁혔음
- 최근의 contrastive한 방식은 일종의 instance discrimination task로서 이미지 변형에 invariance하면서 서로 다른 이미지를 구분할 수 있는 표현을 학습함
- 이때 positive pairs와 negative pairs가 가능한 많이 필요한데 모든 이미지에 대해 discrimination을 수행하는 것은 불가능하므로 비교 샘플의 수를 배치사이즈로 제한하는 등의 approximation이 제안되었음
- Clustering 기반의 방법은 이러한 approximation을 위한 또다른 해결책이 될 수 있음: 각 이미지에 대한 discrimination을 수행하는 것이 아니라 유사한 피처 기반으로 묶은 클러스터 간의 discrimination 수행
- 일반적인 clustering 기법은 전체 image dataset의 image features를 clustering하고 cluster code(clustering numbering)을 부여하는 방식인 offline 방식
- 이러한 방법은 학습을 진행할때 feature를 업데이트하기 위해 전체 dataset을 반복적으로 input하기 때문에 target이 계속 변하는 online 학습에는 시간적 문제때문에 실용적이지 않음
- 따라서, 본 논문에서는 online으로 cluster features를 업데이트하면서 같은 이미지의 서로 다른 views에 대해서는 일관성을 유지하도록 하는 online clustering-based contrastive method을 제안함 (SwAV)
- SwAV는 크고 작은 배치 사이즈 모두에서 잘 작동하고 memory bank나 momentum encoder 등이 필요없음

## 접근법
- Clustering assignments를 통해 다양한 이미지 views를 contrasting하되 이미지 피처 간의 constrastive를 통해 consistency를 유지
- 먼저 같은 이미지로부터 augmented된 두 이미지에 대해 encoding features를 추출
- Encoding features로부터 K개의 trainable cluster prototype vectors로 매핑되는 codes를 계산
- 이때 하나의 배치에 대해 각 입력이 K개의 cluster로 균등하게 할당되도록 함
- Encoding features와 cluster prototype vectors의 softmax를 통해 정의되는 targets에 대해 codes와의 cross-entropy로 loss를 계산
- 이때 각 codes는 대응하는 counterpart view의 targets를 예측하도록 swap되어 진행됨
- Feature encoder와 cluster protoype vectors는 동시에 훈련됨
- 만약 배치 사이즈가 K보다 작아 모든 cluster로 균등하게 할당될 수 없다면 이전 배치의 데이터 일부를 재사용하고 현재 배치의 데이터에 대해서만 loss 계산하는 방법으로 진행

## 실험결과
- Linear evaluation에서 MoCov2 SimCLR을 포함한 다른 self-supervised 방법들의 성능을 능가하였으며, 같은 백본의 fully supervised 성능과의 격차를 매우 줄임
- Fully supervised 모델보다 다른 분류 데이터셋 및 object detection 성능이 좋으므로 SwAV는 더욱 transferable한 표현을 학습한다는 것을 증명
- 작은 배치사이즈에서 SimCLR 및 MoCov2보다 좋은 성능을 달성함
  
## 의견
- DeepCluster 및 SeLa와 함께 대표적인 clustering 기반의 self-supervised learning method 중 하나
- 비교 방법론으로 사용해볼만 함