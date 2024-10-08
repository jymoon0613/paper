# Xception: Deep Learning with Depthwise Separable Convolutions (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Chollet_Xception_Deep_Learning_CVPR_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/chollet2017xception.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN은 컴퓨터 비전 분야에서 큰 성공을 거두었으며, 여러 네트워크 구조가 성능 개선을 위해 제안됨
- 그 중에서도 Inception architecture는 2014년 GoogLeNet을 통해 처음 등장하였으며 (Inception V1), Inception V2, Inception V3, Inception-ResNet을 거쳐 발전되어 왔음
- Inception은 Network-In-Network 아키텍처를 따르며 ImageNet 데이터셋에 가장 성능이 좋은 구조 중 하나임
- Inception architecture 혹은 Inception 모델은 Inception modules을 여러개 쌓아 올려서 완성됨
- Inception module은 기존의 일반적인 convolution layer가 3D 공간(width, height, channel)에서의 filters를 학습하는 것을 분해하여 cross-channel correlations과 spatial correlations을 명시적으로 고려하도록 함
- 즉, Inception module의 기본 가정은 cross-channel correlations과 spatial correlations이 확실히 구분되어 있으므로 동시에 매핑하지 않는 것이 바람직하다는 것
- 이러한 가정을 극도로 일반화하면 Inception module은 depthwise-separable convolution과 거의 같아지게 됨
- 따라서, 본 연구에서는 Inception 계열 모델의 성능 향상을 위해 Inception module을 depthwise-separable convolution으로 교체하고자 함 (Xception)

## 접근법
- Xception 구조는 depthwise separable convolution 레이어와 residual connection을 선형적으로 쌓은 구조

## 실험결과
- ImageNet과 JFT 데이터셋을 사용한 이미지 분류 태스크에서 성능 검증
- 두 데이터셋에서 Inception V3의 성능을 능가함

## 의견
- /