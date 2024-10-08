# Very Deep Convolutional Networks for Large-Scale Image Recognition (ICLR, 2015)

[논문 링크](https://arxiv.org/abs/1409.1556)

<p align="center">
    <img width="400" alt='fig1' src="../img/simonyan2014very.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Convolutional neural networks (CNN)은 이미지 및 비디오 인식 분야에서 좋은 성능을 기록함
- CNN이 CV에서 대세가 되면서 기본적인 CNN 아키텍처를 수정하여 성능 향상을 도모하는 연구들이 많이 제안되었음
- 본 논문에서는 기존의 구조에 더 많은 convolutional layers를 추가하는 방식으로 네트워크의 깊이(depth)를 증가시켜 성능 향상을 이끌어냄
- 추가되는 convolutional layers는 3x3 크기의 매우 작은 filter로 구성되어 있기 때문에 깊이 증가는 네트워크가 실행 가능한 수준으로 구현 가능함

## 접근법
- CNN의 깊이를 증가시킬 때의 성능 개선을 측정하기 위해 같은 디자인으로부터 일부 레이어 및 요소를 추가한 모델을 각각 정의함 (모델의 깊이만 서로 다름)
- 11개의 layer(8개의 conv. layer + 3개의 fc layer)를 사용하는 모델(VGG-11)에서 19개의 layer(16개의 conv. layer + 3개의 fc layer)를 사용하는 모델(VGG-19)까지 정의함
- 11x11이나 7x7 등의 filter size를 사용하는 기존의 모델들에 비해 3x3의 매우 작은 filter size를 사용하기 때문에 레이어 증가에 따른 깊이 증가에 비해 파라미터 수의 증가는 작음
- 물리적으로 3x3 conv. layer 2개를 연속적으로 사용하는 것은 5x5 conv. layer 하나를 사용하는 것과 같으며, 3개를 연속적으로 사용하는 것은 7x7 conv. layer 하나를 사용하는 것과 같음
- 그렇다면 왜 5x5나 7x7 conv. layer를 사용하는 것보다 여러 개의 3x3 conv. layer를 사용하는 것이 좋은가?
> - (1) 더 많은 conv. layer를 사용하는 것은 더 많은 비선형성을 지닌다는 것을 의미하며 이는 더 좋은 분류 정확도에 기여할 수 있음
> - (2) 파라미터 수를 줄일 수 있음: 만약 입력과 출력 모두 C개의 채널로 구성된 3개의 3x3 conv. layer를 쌓는다면 파라미터 크기는 $3(3^2C^2)=27C^2$이지만, 같은 물리적 계산을 위해 7x7 conv. layer를 사용하는 경우 파라미터 크기는 $7^2C^2=49C^2$임. 
> - 즉, 3x3 conv. layer를 여러 개 쌓는 것은 7x7 conv. layer에 일종의 regularization을 수행하는 것과 같으며, 추가적인 비선형성을 기대할 수 있다는 장점도 있음
- 1x1 conv. layer의 추가는 receptive field에 영향을 주지 않고 비선형성을 추가해줄 수 있음 (VGG-16)

## 실험결과
- 네트워크 깊이가 깊어질수록 모델의 성능이 좋아졌음
- ImageNet chanllenge의 SOTA 성능을 달성
- 특히 구조를 크게 변경시키지 않고 깊이만 깊게 설계했는데 좋은 성능을 거둠

## 의견
- /