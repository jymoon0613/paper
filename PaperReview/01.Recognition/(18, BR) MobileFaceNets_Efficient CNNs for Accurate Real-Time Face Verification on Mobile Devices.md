# MobileFaceNets: Efficient CNNs for Accurate Real-Time Face Verification on Mobile Devices (BR, 2018)

[논문링크](https://arxiv.org/abs/1804.07573)

<p align="center">
    <img width="800" alt='fig1' src="../img/chen2018mobilefacenets.png?raw=true"></br>
    <em><font size=2>A typical face feature embedding CNN and the receptive field (RF).</font></em>
</p>

## 연구목적
- 앱 로그인, 결제, 잠금해제 등 모바일 환경에서의 face verification이 중요해지고 있지만, 기존의 face verification 모델들은 깊은 CNN 구조를 사용하고 있기 때문에 모바일 환경에서 사용하기에 적합하지 않음
- 본 논문에서는 모바일 환경을 위한 가볍고 빠른 face verification 모델인 MobileFaceNet을 제안함

## 접근법
- MobileNetV2를 백본으로 사용하고, ArcFace loss를 사용하여 훈련시킴
- MobileNetV2를 face verification에 더 적합하게 만들기 위해 global average pooling(GAP) 대신 global depthwise convolution(GDConv)을 개발하여 적용함
- GDConv는 MobileNetV2의 마지막 feature map을 구성하는 각 요소를 그 중요성에 따라 서로 다르게 처리하는 역할을 함
> - $W\times{H}\times{M}$의 feature map에 $W\times{H}\times{M}$의 depthwise convolution을 적용하여 $1\times{1}\times{M}$의 face features를 얻음
## 실험결과
- MobileFaceNet은 매우 가볍고 빠르지만 기존의 CNN SOTA 모델과 거의 비슷한 성능을 기록함

## 의견
- /