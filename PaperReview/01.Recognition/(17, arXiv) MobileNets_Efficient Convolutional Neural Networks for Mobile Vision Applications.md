# MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications (arXiv, 2017)

[논문 링크](https://arxiv.org/abs/1704.04861)

<p align="center">
    <img width="400" alt='fig1' src="../img/howard2017mobilenets.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN의 등장 이후 정확도를 높이기 위해 여러 매우 깊고 복잡한 네트워크 구조들이 제안되어 왔지만 모델의 사이즈와 추론 및 학습 속도에 대한 고려는 부족
- 실제 세계의 어플리케이션에서 비전 태스크는 대부분 제한된 하드웨어 자원 하에서 동작해야 하며, 따라서 모델의 사이즈 및 속도에 대한 고려가 필수적임
- 본 연구에서는 모바일 및 임베디드 비전 어플리케이션에 적용 가능한 매우 작고 빠른 네트워크 구조를 제안함 (MobileNets)

## 접근법
- MobileNet은 depthwise deparable convolutions에 기반함
- Depthwise deparable convolutions은 기존의 convolution 연산을 depthwise convolution과 pointwise convolution(1x1 convolution)으로 분해하는 convolution 연산
- Depthwise convolution으로 각 입력 채널별 하나의 필터를 적용함
- Pointwise convolution으로 depthwise convolution의 아웃풋을 combine함
- 이러한 분해(factorization)는 모델의 파라미터 및 연산량을 크게 줄이는 효과가 있음
- 모델의 사이즈를 더욱 줄이기 위해 width parameter인 alpha를 제안 (각 레이어의 입력 및 출력 채널 수를 일괄적으로 조정)
- 또한, 모델의 computational cost를 더욱 낮추기 위한 resolution multiplier rho를 제안 (각 레이어의 입력 및 출력 resolution을 일괄적으로 조정)

## 실험결과
- ImageNet 데이터셋으로 평가
- Depthwise separable convolution을 사용하는 경우 일반 convolution을 사용할 때보다 파라미터 수가 약 7배 정도 감소하지만 정확도는 약 1% 포인트 정도만 감소함
- alpha가 작아질수록 정확도가 감소하지만 파라미터 수도 크게 감소
- rho가 작아질수록 정확도가 감소하지만 연산량도 크게 감소
- MobileNet은 VGG-16보다 약 32배 작고 27배 정도 연산량이 적지만 거의 비슷한 성능을 보여줌
- Object detection, fine-grained visual classification을 포함한 여러 태스크에서의 성능도 평가

## 의견
- /