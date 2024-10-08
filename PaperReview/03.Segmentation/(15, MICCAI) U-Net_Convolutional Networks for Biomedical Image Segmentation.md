# U-Net Convolutional Networks for Biomedical Image Segmentation (MICCAI, 2015)

[논문 링크](https://link.springer.com/chapter/10.1007/978-3-319-24574-4_28)

<p align="center">
    <img width="600" alt='fig1' src="../img/ronneberger2015u.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN은 많은 visual recognition tasks에서 좋은 성능을 보여주었지만 이는 주로 classification task에서의 성능 향상에 집중되어 있음
- 하지만 biomedical image processing과 같은 분야에서는 단일 class 분류보다는 localization (각 픽셀에 class label을 부여)과 같은 task가 더욱 중요할 수 있음
- 본 연구에서는 segmentation task를 위한 fully convolutional network 구조를 제안하며, 제안된 모델은 매우 적은 학습 데이터만 가지고도 더 좋은 segmentation 성능을 보여줌 
- 넓은 범위의 이미지 픽셀로부터 의미정보를 추출하고 의미정보를 기반으로 각 픽셀마다 객체를 분류하는 U-Net 아키텍처 제안

## 접근법
- U-Net 구조는 크게 3가지로 구분됨
> - 점진적으로 넓은 범위의 이미지 픽셀을 보며 의미정보(Context Information)을 추출하는 수축 경로(Contracting Path)
> - 의미정보를 픽셀 위치정보와 결합(Localization)하여 각 픽셀마다 어떤 객체에 속하는지를 구분하는 확장 경로(Expanding Path)
> - 수축 경로에서 확장 경로로 전환되는 전환 구간(Bottle Neck)
- 입력 이미지는 수축 경로를 따라 하나의 피처 벡터로 압축되고, 이후 pooling 연산을 upsampling 연산으로 대체한 확장 경로를 통해 피처 벡터의 resolution을 다시 증가시킴 (Encoder-Decoder)
- 확장경로 진행 시 skip connection을 통해 수축경로에서 생성된 context 정보를 결합해줌으로써 high resolution features를 반영한 더 정확한 예측 가능
- 네트워크의 최종 output은 segmentation map
- 이미지 경계 부분의 segmentation을 수행하기 위해 image mirroring을 통해 입력 이미지를 확장하고 부족한 context를 보충
- Deformation, scale 등에 대한 강건성을 보장하기 위해 여러 augmentation 기법 적용
- 또한 객체 간의 경계를 더욱 잘 학습하기 위한 weighted loss를 정의

## 실험결과
- Biomedical segmentation task에서 좋은 성능을 보임

## 의견
- /