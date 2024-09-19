# A Unified Multi-scale Deep Convolutional Neural Network for Fast Object Detection (ECCV, 2016)

[논문링크](https://link.springer.com/chapter/10.1007/978-3-319-46493-0_22)

<p align="center">
    <img width="700" alt='fig1' src="../img/cai2016unified.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Faster R-CNN의 RPN은 backbone의 마지막 feature maps에서 region proposals를 생성함
- 이처럼 single receptive field에서만 region proposals를 생성하게 되면 objects의 scale variance에 대응하기 힘듦
- 따라서, 본 논문에서는 이러한 문제를 해결하기 위해 multi-scale CNN(MS-CNN)을 제안함
> - Faster R-CNN과 마찬가지로 RPN과 Fast R-CNN 모듈로 구성되어 있음
> - 단, 특정 scale ranges의 objects에 집중하고 있는 여러 output layers에서 각각 detection을 수행하도록 함
> - 또한, feature upsampling을 사용하여 small objects에 대한 detection 정확도를 개선하고자 함

## 접근법
- 먼저 MS-CNN의 proposal network는 backbone의 여러 layers에서 각각 region proposal을 수행함
> - $H\times{W}$의 입력 이미지에 대해 $\{HW/8^2, HW/16^2, HW/32^2, HW/64^2\}$ resolution의 feature maps에 각각 region proposal module을 부착
- 이때 4개의 region proposal module은 각자에게 할당된 scale 범위 내에 있는 training samples에 대해서만 학습됨
- 또한, 4개의 region proposal module에 detection module도 부착함
> - 각 detection module은 region proposal module이 생성하는 region proposals에 대해 RoI pooling을 사용하여 고정된 크기의 features를 추출함
- 이때 feature maps는 deconvolutional layer를 거쳐 resolution이 2배 증가하게 되고, 증가된 feature maps에 RoI pooling이 적용됨
> - 입력 이미지의 resolution을 키우는 방법과 비교했을 때, feature maps의 resolution을 증가시키는 방법은 적은 연산량 증가로 small objects에 대한 detection 정확도를 효과적으로 개선시킬 수 있음

## 실험결과
- KITTI, Caltech Pedestrian 데이터셋에서 모델을 평가함
- RPN, EdgeBoxes 등과 비교했을 때 MS-CNN은 더 적은 proposals로도 더 좋은 proposal generation 성능을 달성 
- KITTI, Caltech Pedestrian에서 SOTA detection 성능 달성

## 의견
- / 