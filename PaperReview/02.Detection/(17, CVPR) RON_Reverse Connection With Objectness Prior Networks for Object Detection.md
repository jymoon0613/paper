# RON: Reverse Connection with Objectness Prior Networks for Object Detection (CVPR, 2017)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Kong_RON_Reverse_Connection_CVPR_2017_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/kong2017ron.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 region-based detection methods(i.e., Faster R-CNN)과 region-free detection methods(i.e., YOLO)를 적절히 결합한 detection 프레임워크를 제안하고자 함
- 특히 다음과 같은 두 문제에 집중함:
> - (i) Multi-scale object localization: reverse connection을 제안하여 objects가 대응되는 network scales에서 detect될 수 있도록 함
> - (ii) Negative space mining: negative mining의 search space를 줄이기 위해 convolutional feature maps 상에서의 objectness prior을 활용함
> - Reverse connection with objectness prior networks (RON)

## 접근법
- VGG-16을 backbone으로 사용함
- Reverse connection은 서로 다른 scale의 정보를 결합하기 위해 제안됨
> - Highly-abstracted information과 ine-grained details를 결합
> - Top-down 방식으로 정보가 결합됨
- Reverse connection block은 deconvolutional layer와 convolutional layer로 구성됨
> - 상위 layer의 feature maps에 deconvolution을 적용하여 upsampling하고, 현재(하위) layer의 feature maps에 convolution을 적용한 뒤 element-wise addition함
- Backbone의 4개 stage에서 reverse connection block을 사용하여 multi-scale의 정보가 fusion된 4개의 feature maps를 추출함
- 4개의 feature maps 각각에서 detection을 수행함
- RON은 각 pixel position마다 10개의 anchor boxes를 사용하고, 4개의 feature maps 각각에서 detection을 수행하므로 positive samples에 비해 negative samples의 수가 매우 커지게 됨
- 이에 objectness prior을 사용하여 negative samples의 수를 효과적으로 줄임
> - 각 stage의 feature maps에 convolutional layers와 softmax function을 추가로 연결하여 objectness prior maps를 생성하고, 이를 confidence score처럼 사용함

## 실험결과
- Fast R-CNN, Faster R-CNN, SSD와 비교했을 때 더 좋은 detection 성능을 기록함
- 특히 RON은 small-sized objects에 대한 detection 성능이 좋았음

## 의견
- / 