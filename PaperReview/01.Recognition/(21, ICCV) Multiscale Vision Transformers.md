# Multiscale Vision Transformers (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Fan_Multiscale_Vision_Transformers_ICCV_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/fan2021multiscale.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Visual processing에서 (1) 모델의 layer를 거칠수록 spatial resolution을 줄이고, (2) channels의 수는 증가시키는 'hierarchy'의 개념은 예전부터 사용되어 자리잡고 있음
- 유사한 개념으로 multiscale processing을 위한 'pyramid' 전략은 (1) 컴퓨팅 연산을 줄이기 위해 low resolution 상에서 작업을 진행하고 (2) low resolution에서 학습한 context 정보를 high resolution 처리 과정에 반영함
- 본 논문에서는 transformer model을 이러한 multiscale feature hierarchies와 결합하여 여러 visual recognition tasks에서의 성능 향상을 도모함 (Multiscale Vision Transformers (MViT))
- 기존 ViT와 달리 stage가 진행되면서 spatial resolution은 감소하고 channel의 수는 증가함
> - 이는 모델이 초기의 high spatial resolution에서는 가벼운 channel cpapacity와 함께 간단한 low-level visual information을 학습하고, low spatial resolution에서는 spatially coarse하지만 complex한 high-level visual semantics를 학습할 수 있도록 함
- Video recognition tasks에 집중함 (image classification도 적용 가능함)

## 접근법
- MViT의 핵심은 pooling을 통해 입력의 resolution을 줄이면서 channel capacity를 확장하는 것
- Multi Head Pooling Attention (MHPA)는 Multi Head Attention (MHA)와 마찬가지로 query, key, value로 output을 linear projection함
- 하지만 self-attention 이전, pooling operator에 의해 query, key, value가 pooling됨 (reshape -> pooling -> flatten)
- 이후 pooled된 query, key, value를 기반으로 self-attention이 진행됨
- Convolution layer를 통해 pooling 수행
- Stage가 진행될수록 channel이 expansion됨

## 실험결과
- MViT는 video recognition에서 ImageNet 21K pretrainig을 한 또 다른 Transformer 기반 vidio recognition 모델인 VTN이나 ViViT보다 월등한 성능을 기록함
- Image recognition에서도 좋은 성능 기록함

## 의견
- CvT와 공통점이 많음