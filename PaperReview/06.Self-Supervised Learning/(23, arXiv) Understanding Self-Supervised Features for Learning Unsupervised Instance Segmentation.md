# Understanding Self-Supervised Features for Learning Unsupervised Instance Segmentation (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2311.14665)

<p align="center">
    <img width="700" alt='fig1' src="../img/engstler2023understanding.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 instance segmentation을 위한 self-supervised learning(SSL)을 다룸

## 접근법
- MAE, DINO를 포함한 여러 SSL method로 Transformer를 훈련시킴
- 이후 pseudo-mask 기반의 segmentation을 수행하여 각 SSL method의 효과성, 즉, instanceness를 확인
> - Instanceness: semantic categories를 넘어 object instances 간의 차이를 식별할 수 있는 정도
> - 훈련된 Transformer의 마지막 self-attention block의 keys로부터 features를 추출하고, k-way clustering을 수행
> - 그 결과 k개의 meaningful regions에 대한 masks를 생성할 수 있음
> - 실험 결과 MAE로 훈련되었을 때 모델의 instanceness가 가장 좋았음
- 이를 기반으로 mask proposals를 생성하여 segmentation network를 학습시키는 간단한 방법을 구현함
- 먼저, MAE로 훈련된 Transformer가 meaningful mask candidates를 생성함
> - 생성 방법은 이전과 같음
- 이후 noisy한 mask candidates(e.g., background regions)를 필터링하기 위한 saliency-based masking을 수행
- 이는 DINO로 훈련된 Transformer의 features에 k=2의 clustering을 적용하여 수행됨
> - 이후 IoU 및 NMS를 적용하여 salient한 mask proposals만 남김
- 최종적으로 생성된 mask proposals는 instance segmentation network를 훈련시키는 데 사용됨
> - SOLOv2 아키텍처를 사용
> - 네트워크는 오직 생성된 pseudo-maks만 사용하여 훈련됨

## 실험결과
- MAE로 훈련된 Transformer를 mask proposal generator로 사용할 때 성능이 가장 좋았음
> - MAE의 pixel-wise reconstruction objective가 lower semanticity(i.e., fine-grained details)를 학습하는 데 효과적이며, 따라서 유사한 semantic category를 그룹화하는 능력이 좋음

## 의견
- /