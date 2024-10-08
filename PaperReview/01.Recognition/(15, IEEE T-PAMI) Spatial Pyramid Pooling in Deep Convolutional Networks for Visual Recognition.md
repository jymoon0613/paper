# Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition (IEEE T-PAMI, 2015)

[논문링크](https://ieeexplore.ieee.org/abstract/document/7005506)

<p align="center">
    <img width="400" alt='fig1' src="../img/he2015spatial.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN은 여러 vision tasks에서 좋은 성능을 보였지만 입력의 크기가 고정되어야 한다는 단점이 있었음
> - Cropping, warping의 방식으로 고정된 크기의 입력을 사용함
> - 하지만 cropping은 정보의 손실을 야기하며, warping은 예기치 못한 geometric distortion을 야기함
> - 또한, 고정된 크기의 image scale은 object scales가 다양한 detection tasks에서 문제가 될 수 있음
- CNN이 고정된 크기의 입력이 필요한 이유는 fully connected layer 때문임 (convolution layer는 어느 크기의 feature maps도 생성할 수 있음)
- 따라서, 본 논문에서는 spatial pyramid pooling (SPP) layer를 제안하여 네트워크의 고정된 크기 입력 제한을 제거하고자 함 (SPP-Net)
> - SPP layer는 convolutional layer의 최상단에 위치함
> - Feature pooling을 통해 고정된 크기의 output을 생성하고 fully connected layer에 전달함

## 접근법
- CNN의 마지막 pooling layer를 spp layer로 대체함
- 먼저 spatial bin의 크기 M를 정함 (i.e. 50 = [6x6, 3x3, 2x2, 1x1], 30 = [4x4, 3x3, 2x2, 1x1])
- 설정된 bin과 각 grid에서 max pooling을 적용함
- SPP layer의 최종 output은 kM 차원의 벡터 (k: channel 수)
- 만약 M = 1 = [1x1]이면 global max pooling과 일치함
- 입력 사이즈가 다양하므로 SPP layer로 입력되는 feature map의 크기도 다양함
- 이때 pooling의 window size와 stride 만을 조절하여 출력 크기를 결정
> - window size = ceiling(feature map size / pooling size)
> - stride = floor(feature map size / pooling size) 
> - 어떠한 feature map 크기가 오더라도 고정된 pyramid size를 얻을 수 있음

## 실험결과
- SPP-Net을 사용했을 때 classification 성능 개선
- Detection task에서는 기존 R-CNN 기반으로 적용함
> - 기존 R-CNN은 하나의 이미지에서 약 2,000개 정도의 region proposals을 생성하고 각각을 CNN으로 피처를 추출함 (time-consuming, bottleneck)
> - 하나의 이미지에서 하나의 피처맵만을 추출함
> - 이후 feature map 상의 각각의 candidate window에 spatial pyramid pooling을 적용하여 고정된 크기의 feature vector를 각각 생성함
- R-CNN보다 일부 평가지표에서 좋은 성능을 보였으며 속도를 크게 개선함

## 의견
- /