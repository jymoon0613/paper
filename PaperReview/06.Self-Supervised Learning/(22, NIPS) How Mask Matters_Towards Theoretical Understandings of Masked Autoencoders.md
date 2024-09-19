# How Mask Matters: Towards Theoretical Understandings of Masked Autoencoders (NIPS, 2022)

[논문링크](https://arxiv.org/abs/2210.08344)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhangmask.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 MAE에서 masking의 역할과 중요성에 대해 탐구해보고자 함
- 분석 결과를 바탕으로 기존 MAE에서 feature diversity를 향상시킨 uniformity-enhanced MAE(U-MAE)를 제안함

## 접근법
- MAE는 그 이름에서 알 수 있듯이 mask와 autoencoder로 구성됨
- 하지만 autoencoder 자체는 MAE의 성능에 큰 영향을 주지 않음
> - MAE에서 masking을 사용하지 않고 훈련시키면 downstream 성능이 극단적으로 하락함
> - 즉, 마스킹 매커니즘이 MAE의 핵심임
- MAE의 작은 reconstruction loss는 MAE가 masked view와 unmasked view의 latent features를 잘 align하고 있다는 것을 의미함
> - Contrastive learning의 positive pairs가 하는 역할과 비슷
- MAE는 dimensional feature collapse가 발생할 수 있음
> - 학습된 features가 low-dimensional subspace에 존재
> - Representation power를 제한함
> - 이러한 문제를 해결할 수 있는 명시적인 regularization이 필요
- MAE의 feature diversity를 증진시키기 위해 uniformity-enhanced MAE(U-MAE) loss를 제안
> - Contrastive learning의 uniformity loss에서 영감을 받음
> - 기존 MAE loss에 uniformity loss를 추가
> - Random unmasked views 간의 feature similarity를 encourage함

## 실험결과
- CIFAR-10, ImageNet-100, ImageNet-1K에서의 linear probing을 진행한 결과 U-MAE는 기존 MAE보다 전체적으로 개선된 성능을 보임
- SimMIM에 U-MAE loss를 추가하는 경우에도 성능 증가가 있었음

## 의견
- /