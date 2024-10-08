# Object Detection Networks on Convolutional Feature Maps (IEEE T-PAMI, 2016)

[논문링크](https://ieeexplore.ieee.org/abstract/document/7546875)

<p align="center">
    <img width="600" alt='fig1' src="../img/ren2016object.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이전까지의 연구는 주로 feature extractor의 발전에 집중해왔지만, 본 논문에서는 object classifier의 효과적인 설계가 detection 성능을 더 향상시킬 수 있다고 주장함
- Detection 시스템은 주로 이미지로부터 region-independent한 features를 추출하고, region-wise MLPs를 연결하여 detection을 수행하는 과정으로 이루어져 있음
- 본 논문에서는 기존의 region-wise MLPs를 convolution을 포함한 region-wise networks로 대체하고자 함 (networks on convolutional feature maps, NoC)

## 접근법
- ZF 혹은 VGG backbone을 사용하여 주어진 이미지로부터 feature maps를 생성하며, selective search로 찾은 2000개 가량의 region proposals에 대해 RoI pooling을 사용하여 fixed-size features를 추출함
- 추출된 features를 입력으로 하여 여러 NoC 아키텍처를 실험해봄
- NoC를 MLP로만 구성하는 경우 4-layer MLP의 성능이 가장 좋았음
- NoC에 convolution을 추가하는 경우 2개의 convolution과 3-layer MLP를 연결하는 것이 성능이 가장 좋았음
- 또한, NoC에 maxout을 추가해봄
> - Maxout은 주어진 두 features에 대해 element-wise max를 취하여 하나의 features로 aggregate함
> - 하나의 region proposal에 대해 backbone의 서로 다른 scale의 두 feature maps에서 각각 고정된 크기의 features를 추출함
> - NoC는 두 multi-scale features에 대해 maxout을 적용하고, aggregate된 features를 사용함으로써 scale invariance를 증진시킬 수 있음
> - 첫 번째 convolution 이후 maxout을 추가했을 때 성능이 가장 좋았음

## 실험결과
- 실험 결과로부터 강력한 feature extractor(backbone)만큼 효과적인 object classifier 설계도 중요함을 증명함
- NoC를 사용했을 때 Fast R-CNN, Faster R-CNN 모두 유의미한 성능 향상이 있었음

## 의견
- / 