# Learning Imbalanced Data with Vision Transformers (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2212.02015)

<p align="center">
    <img width="500" alt='fig1' src="../img/xu2023learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 여러 vision tasks에서 좋은 성능을 보여주었지만, 이러한 ViT가 좋은 성능을 내기 위해서는 엄청난 양의 balanced 데이터가 필요함
- 하지만 현실 세계의 데이터는 보통 class-imbalance한 경우가 많음: Long-tailed recogntion(LTR)
> - 소수의 클래스 샘플이 데이터 분포의 대부분을 차지하고(head) 대다수의 클래스 샘플은 매우 적게 분포함(tail)
- 따라서, 본 논문에서는 ViT를 LT 데이터로 학습시키는 방법에 대해서 탐구함
- Class-imbalance 해결을 위해 feature extraction을 향상시키는 방법이 있음
> - Mixup, remix를 사용한 supervised manners
> - Contrastive learning(CL) 기반의 self-supervised learning
> - SSL이 class imbalance 상황에서 supervised methods보다 더 robust함
- 하지만, CL은 추가적인 연산량 및 메모리가 많이 필요하며, 모델 수렴의 어려움이 있음
- 따라서, 본 논문에서는 masked generative pretraining(MGP)와 balanced fine tuning(BFT)를 사용하여 imbalanced data로 ViT를 학습시키는 방법을 제안함(LiVT)
> - MGP는 MAE처럼 masked된 이미지 토큰을 reconstruction하여 feature extraction을 향상시키는 방법임
> - BFT는 훈련 모델의 downstream head를 balanced binary cross-entropy(Bal-BCE) loss를 기반으로 tuning하여 성능을 향상시키는 방법임

## 접근법
- 우선 다음과 같은 이유로 ViT의 feature extraction을 향상시키기 위해 MGP를 사용하여 모델을 훈련시킴
> - ViT를 바로 labeled dataset으로 훈련시키는 것은 비용이 크고 수렴이 어려움
> - MGP는 다른 SSL 방법인 CL or SCL(supervised contrastive learning)에 비해 class instance의 수의 영향이 적음
- 이후 MGP로 훈련된 모델의 head를 BFT phase에서 fine-tuning함
- 일반적으로 softmax + cross-entropy(CE)가 일반적인 tuning 패러다임이지만, 최근의 연구 결과에 따르면 binary cross-entropy(BCE)가 ViT의 성능 향상에 더 뛰어나며, mixup과 같은 방법을 같이 사용하기에도 용이하다는 것이 밝혀짐
- LTR에서는 balanced cross-entropy(BCE)가 기존 CE의 성능을 유의미하게 개선하였지만, 이러한 balancing을 BCE에 바로 적용하기에는 어려움 (logit bias 때문)
- 따라서, 이러한 logit bias에 대한 고려를 추가한 balanced binary cross-entropy(Bal-BCE)를 정의하여 fine-tuning 과정에서 사용함

## 실험결과
- CIFAR-10/100-LT, ImageNet-LT, iNaturalist 2018, Places-LT에서 평가를 진행함
- ViT를 제안하는 방법으로 scratch로 훈련시켰음에도 이전의 LT 방법들을 능가하여 SOTA 성능 달성

## 의견
- /