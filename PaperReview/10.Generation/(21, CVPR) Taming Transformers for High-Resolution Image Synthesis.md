# Taming Transformers for High-Resolution Image Synthesis (CVPR, 2021)

[논문링크](https://arxiv.org/abs/2012.09841)

<p align="center">
    <img width="600" alt='fig1' src="../img/esser2021taming.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN과 Transformer를 동시에 사용하여 visual world의 compositional nature를 모델링할 수 있음
> - CNN을 사용하여 context-rich visual parts의 codebook과 그것들의 global compositions을 학습함
> - Visual parts 간의 interactions를 모델링하기 위해 Transformer 사용
- 이전에도 Transformer를 적용하여 image generation tasks를 수행한 연구가 있었지만 self-attention의 quadratic complexity 때문에 64x64라는 작은 resolution에만 적용되었음
- 본 논문에서는 pixel 단위의 interactions를 모델링하는 것이 아닌 visual parts 단위의 interactions를 모델링함으로써 Transformer의 complexity를 낮춤
> - Codebook에 저장된 visual parts의 composition으로 image를 표현

## 접근법
- VQ-VAE와 마찬가지로 하나의 이미지를 그것을 구성하는 visual parts의 codebook entries로 표현함
> - $N$개의 vector로 구성된 learnable codebook $Z$를 정의
> - CNN encoder로 입력 이미지의 feature vectors $\hat{Z}$를 추출
> - 각 feature vector $\hat{z}_i$와 codebook features $z_i$의 euclidean distance를 비교하여 가장 가까운 codebook feature의 index를 배정 (quantization)
- Index로 quantization된 feature map $Z_q$를 CNN decoder에 입력하여 reconstruction image를 생성
- 생성된 이미지를 discriminator에 넣어 patch 단위로 real 인지 fake 인지 판단
- Loss functions는 reconstruction loss와 VQ loss, commitment loss로 구성됨
> - Reconstruction loss는 실제 이미지와 생성된 이미지 간의 차이를 구하는 loss로 생성된 이미지가 실제 이미지와의 차이가 없도록 만드는 것을 목적으로 함
> - VQ loss는 codebook만 update 하는 loss로 codebook vector가 encoder의 출력과 비슷하게 만들도록 하는 목적
> - Commitment loss는 encoder만 update 하는 loss로 encoder의 출력이 codebook vector와 가까운 값을 출력하는 것이 목적인 loss
- 이때 codebook의 richness를 개선하기 위해 기존 VQ-VAE와 달리 discriminator and perceptual loss를 사용하는 VQ-GAN을 제안
> - 기존 L2 distance 기반 reconstruction loss를 perceptual loss로 변경
> - Real(입력) image와 fake(생성) image를 VGG discriminator에 입력하고 두 network-level feature maps의 L2 distance를 최소화
- 이후 Transformer를 image synthesis에 활용
> - Transformer는 $Z_q$의 codebook index를 auto-regressive하게 예측 (이전 index를 바탕으로 다음 index를 예측)
> - 예측한 index의 patch vector를 codebook에서 가져오면서 이미지 생성
- Conditional한 생성도 가능
> - Condition은 depth map이나 semantic segmentation map, keypoint, image class 등이 될 수 있음
> - 각 condition에 대한 codebook을 생성

## 실험결과
- 같은 step, 같은 time으로 훈련을 하였을 때 Transformer를 image synthesis에 활용하는 것이 PixelCNN 계열의 아키텍처를 사용하는 것보다 성능이 좋았음
- VQ-GAN은 high-resolution image generation 성능이 매우 뛰어났음

## 의견
- /