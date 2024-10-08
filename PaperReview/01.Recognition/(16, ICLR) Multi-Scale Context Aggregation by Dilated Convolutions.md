# Multi-Scale Context Aggregation by Dilated Convolutions (ICLR, 2016)

[논문링크](https://arxiv.org/abs/1511.07122)

<p align="center">
    <img width="600" alt='fig1' src="../img/yu2015multi.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Semantic segmentation과 같은 vision tasks는 recognition과 달리 dense한 prediction을 요구함
> - Recognition 모델은 여러 pooling 및 subsampling layers를 통해 multi-scale contextual information을 global prediction으로 output함
> - 반면 dense prediction은 multi-scale에서의 contextual reasoning 과정이 필요할 뿐만 아니라 full-resolution output을 요구함
- 본 논문에서는 이러한 dense한 output을 유지하면서 multi-scale의 contextual reasoning 과정을 수행하는 convolutional network module을 제안함
- 제안하는 네트워크는 pooling이나 subsampling을 사용하지 않으며 이러한 과정을 'dilated convolution'으로 대체함
> - Dilated convolution: resolution을 줄이지 않으면서 receptive field를 확장할 수 있도록 해주는 convolution 기법

## 접근법
- 제안하는 네트워크는 dilated convolution에 기반함
- Dilated convolution은 resolution을 줄이지 않으면서 receptive field를 exponential하게 증가시킬 수 있음
- Dilation parameter 조절을 통해 같은 수의 파라미터로도 receptive field를 확장 가능함
- Dense prediction의 성능을 높이기 위해 multi-scale의 contextual information을 통합하는 context module을 추가
- Context module은 C개의 채널을 입력으로 받아 C개의 채널을 출력함
- Context module은 여러 dilation parameter의 dilated convolution으로 구성됨
- Context module의 앞단에는 backbone을 연결
- VGG-16을 기반으로 3 채널 이미지를 입력으로 받아 21 채널의 피처맵을 출력함
- 생성된 피처맵을 context module로 전달함

## 실험결과
- Pascal VOC semantic segmentation tasks에서 좋은 성능을 기록

## 의견
- /