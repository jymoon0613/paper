# Pelee: A Real-Time Object Detection System on Mobile Devices (NIPS, 2018)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2018/hash/9908279ebbf1f9b250ba689db6a0222b-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2018pelee.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Mobile device를 위한 detection system인 PeleeNet을 제안하고 이를 SSD detection 프레임워크와 결합한 Pelee 디자인
  
## 접근법
- Pelee는 DenseNet 구조에 기반하고 있음
> - 서로 다른 scale의 receptive fields를 얻기 위해 Inception module과 유사하게 2-way의 dense layer 사용
> - 추가 연산량 없이 네트워크의 첫 단계에서 feature extraction quality를 개선하는 cost-efficient한 stem block을 설계
> - Input shape에 따라 bottleneck layer의 channel 수를 조절
> - Transition layer의 input/output channel 수를 동일하게 설정
> - DenseNet의 pre-activation 방식을 post-activation 방식으로 변경
- PeleeNet을 SSD의 detection framework와 결합(Pelee)
> - SSD가 6개의 feature maps에서 detection을 수행하는 대신 Pelee는 5개만 사용함
> - 각 feature map가 detection branch로 전달되기 전 residual block을 지나도록 함

## 실험결과
- MobileNet 기반의 SSD보다 아주 조금 느렸지만 detection 성능은 크게 개선됨
- iPhone8과 같은 real-device에서의 평가에서는 SSD+MobileNet보다 빨랐음

## 의견
- / 