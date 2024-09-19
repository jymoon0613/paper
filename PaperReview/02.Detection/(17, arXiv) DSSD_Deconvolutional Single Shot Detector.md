# DSSD: Deconvolutional Single Shot Detector (arXiv, 2017)

[논문링크](https://arxiv.org/abs/1701.06659)

<p align="center">
    <img width="700" alt='fig1' src="../img/fu2017dssd.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 SSD에 당시의 SOTA CNN 구조인 ResNet-101 backbone을 연결하고, context 정보 학습을 위한 deconvolutions를 추가로 도입하여 detection 성능을 개선하고자 함 (DSSD)
> - 기존의 SSD는 VGG를 backbone으로 사용하였지만, ResNet을 backbone으로 사용했을 때 detection 성능이 향상되는 결과들이 보고된바 있음
> - Deconvolution에 기반한 encoder-decoder, hourglass 구조는 context 모델링에 장점이 있으며 semantic segmentation, human pose estimation에서 활발히 사용되고 있음

## 접근법
- 먼저, SSD의 backbone을 기존의 VGG에서 ResNet-101로 변경함
> - 하지만, backbone 변경만으로는 유의미한 성능 향상이 발견되지 않았음
- 다음으로, 추출된 features에 대해 object classification과 bbox regression을 수행하는 prediction module을 일부 변경함
> - 기존 SSD의 shallow한 prediction module 대신, 하나의 residual block이 추가된 더 deep한 prediction module을 사용함
- 마지막으로, 기존의 SSD 구조에 몇 개의 deconvolutional layers를 추가하여 hourglass 구조를 만듦
> - SSD layers(SSD에서 backbone의 feature maps에 연결된 추가적인 convolutional layes)의 마지막 output에 총 5개의 deconvolutional layers를 연속적으로 연결
> - 이때 skip-connection을 통해 encoder와 decoder의 정보 흐름을 강화
- SSD layers의 마지막 output feature maps와 5개의 deconvolutional layers의 output feature maps 각각에 prediction module을 연결하여 detection 수행

## 실험결과
- PASCAL VOC2007/2012에서 SSD보다 좋은 성능을 기록하였으며, DSSD와 유사하게 context 정보를 모델링하는 MRCNN, ION 보다도 좋은 성능을 기록함
- DSSD는 SSD와 달리 small-sized objects에 대한 detection 성능이 준수했음
- 속도는 SSD에 비해 살짝 느려졌음
> - VGG보다 layer 수가 많은 ResNet-101 backbone을 사용하고, deconvolutional layers가 추가되었으므로

## 의견
- / 