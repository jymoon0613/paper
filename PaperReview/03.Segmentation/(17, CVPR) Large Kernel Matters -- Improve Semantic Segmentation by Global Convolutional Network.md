# Large Kernel Matters -- Improve Semantic Segmentation by Global Convolutional Network (CVPR, 2017)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Peng_Large_Kernel_Matters_CVPR_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/peng2017large.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Semantic segmentation은 per-pixel classification problem으로 볼 수 있음
> - Object는 적절한 class로 구분되어야 하며 (classification), object를 구성하는 각각의 labeled pixels는 output score map에 적절히 위치하여야 함 (localization)
- 하지만 semantic segmentation을 구성하는 두 tasks (classification and localization)은 보통 상충관계에 있음
> - Classification의 성능을 높이기 위해서는 translation 및 rotation을 포함한 transformation에 invariant한 성질이 필요함
> - 반대로 모든 pixel을 정확히 locate해야 하는 localization은 transformation-sensitive한 성질이 필요함
- 따라서, 본 논문에서는 이러한 두 상충되는 tasks를 동시에 고려하는 segmentation network 아키텍처인 global convolutional network (GCN)을 제안함
- GCN은 두 가지 디자인 원칙을 따름
> - Localization 성능을 보장하기 위해 fully convolutional한 구조를 채택하며 localization information을 제거하는 fully-connected layers 혹은 global pooling layers를 사용하지 않음
> - Classification 성능을 보장하기 위해 large kernel size가 사용되며, 이는 per-pixel classifier와 feature maps를 densely하게 연결하여 여러 transformation에 대한 robustness를 향상시킴

## 접근법
- ResNet을 feature encoder로 사용하며 FCN을 segmentation framework로 사용함
- ResNet의 각 stage에서 multi-scale feature maps가 추출됨
- GCN이 multi-scale semantic score maps를 생성하기 위해 사용됨
> - 이때 lower resolution의 score maps는 deconvolution layer를 통해 higher resoltuion의 score maps 생성에 순차적으로 사용됨
> - 마지막으로 upsampling된 score map이 최종 output이 됨
- 또한, boundary refinement(BR) block을 사용하여 object의 boundary 예측을 강화함

## 실험결과
- PASCAL VOC 2012와 Cityscapes 데이터셋에서 검증
- GCN의 핵심은 큰 kernel size를 사용하는 것
- 이를 검증하기 위해 GCN을 여러 kernel size로 설계하여 평가해봄
> - Kernel size가 증가할수록 성능이 일관적으로 향상되었으며 kernel size가 15일 때 성능이 가장 좋았음
> - Feature encoder의 output feature map이 16x16이므로 15x15의 kernel size는 global convolution이라고 할 수 있음
> - 성능의 증가는 kernel size의 증가로 인한 파라미터 수의 증가로부터 기인한 것은 아님
> - 같은 kernel size를 구현하기 위해 여러 convolution layer를 stacked하는 방법도 있지만, 이러한 방법은 효과가 없었음
- GCN은 classification 성능 개선을 위해 사용되었으며, 그 결과 object의 internal region의 segmentation 성능을 높여주는 효과가 있음
- 반대로, BR block은 object의 boundary region의 segmentation 성능을 높여주는 효과가 있었음
- PASCAL VOC 2012 test set 및 Cityscapes test set에서 SOTA 달성

## 의견
- /