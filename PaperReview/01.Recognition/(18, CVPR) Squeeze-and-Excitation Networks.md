# Squeeze-and-Excitation Networks (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Hu_Squeeze-and-Excitation_Networks_CVPR_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/hu2018squeeze.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN의 발전과 함께 네트워크에 spatial correlations을 포착하는 데 도움이 되는 learning mechanism을 명시적으로 내장함으로써 성능을 향상시키는 연구가 많이 제안됨
> - 대표적으로 Inception module은 네트워크의 각 block이 multi-scale의 피처를 포착하도록 설계하여 성능을 향상시킴 
- 본 연구에서는 squeeze-and-excitation(SE) block을 제안하여 spatial한 측면이 아닌 channel relationship에 집중하여 성능을 향상시키고자 함
> - Convolution features의 채널 간의 interdependencies를 명시적으로 모델링하여 네트워크의 representational power를 향상시킴
> - SE block은 global information을 사용하여 중요한 features를 강조하고, 덜 중요한 features의 영향은 감소시키는 feature recalibarion을 수행함
- SE block을 여러 개 stack하여 SE network 설계

## 접근법
- SE block의 목적은 convolution features에서 중요한 features는 강조하고, 덜 중요한 features는 억제하는 것
- 이는 squeeze와 excitation이라는 두 단계에 걸쳐 이루어짐
- Squeeze 연산은 각 channel unit의 global한 spatial information을 하나의 channel descriptor로 축약함
> - Global average pooling 사용
- 다음으로 channel-wise dependencies를 포착하기 위해 excitation 연산을 진행
- Excitation 연산은 (1) 채널 간의 비선형 관계를 모델링할 수 있을 만큼 유연해야 하며, (2) 상호 배타적이지 않은 관계를 학습하여 여러 채널이 동시에 강조될 수 있도록 해야함
- 이를 위해 sigmoid activation을 사용한 간단한 gating function을 설계
> - FC layer -> ReLU activation -> FC layer -> sigmoid로 구성
- Excitation을 통해 얻은 channel scaling parameters를 feature map에 곱해주어 최종 output 생성
- SE block은 기존의 deep CNN 구조에 쉽게 적용 가능함
- SE block의 추가로 인한 복잡도 증가는 미비함

## 실험결과
- ImageNet competition 2017에서 1위 달성

## 의견
- /