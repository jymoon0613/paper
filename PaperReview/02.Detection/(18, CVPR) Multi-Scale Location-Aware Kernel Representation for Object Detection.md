# Multi-Scale Location-Aware Kernel Representation for Object Detection (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Wang_Multi-Scale_Location-Aware_Kernel_CVPR_2018_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/wang2018multi.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 RoI pooling stage에서 high-order statistics representations를 사용하는 multi-scale location-aware kernel representation(MLKP)을 제안하여 detection 성능을 개선하고자 함
> - Backbone의 서로 다른 layers에서 feature maps를 추출한 뒤 하나의 feature maps로 concat함
> - 이후 해당 feature maps에서 high-order statistics를 계산함
- High-order statistics를 직접 계산하는 것은 상당한 연산량과 메모리가 요구되기 때문에 polynomial kernel approximation을 사용하여 이를 해결함
> - 'Low-dimensional' high-order representations
> - Kernel representation은 1x1 convolution과 element-wise product로 근사됨
- 또한, 서로 다른 locations의 contributions를 측정하는 trainable locationweight structure를 디자인하여 추출된 feature representation이 location sensitive한 특성을 가질 수 있도록 함

## 접근법
- Faster R-CNN을 베이스라인으로 사용함
- VGG-16 backbone의 서로 다른 layers에서 feature maps를 추출하고, upsampling 및 element-wise sum을 사용하여 multi-scale 정보를 포함하고 있는 single feature maps를 생성함
- 생성된 feature maps로부터 location-aware polynomial kernel representation을 추출함
- 먼저, polynomial kernel approximation을 통해 high-order statistics를 쉽게 계산할 수 있음
> - Feature maps에 1x1 convolution을 적용한 뒤, 모든 output feature maps를 element-wise product하여 high-order kernel representation을 획득함
> - 마지막으로, global sum-pooling을 적용하여 orderless representation을 획득함
- 하지만 orderless representation은 location information이 없으므로 detection task에 적합하지 않고, 특히 bbox regression에 불리하므로 global sum-pooling operation을 사용하지 않음
> - 1x1 convolution과 element-wise product는 location information을 보존함
- 이때 high-order kernel representation에 location weight를 추가하여 object localization에 유용한 feature maps regions가 더 강조될 수 있도록 함
- Feature maps를 learnable CNN block으로 입력하여 weight maps를 추출하고, 이를 resize한 뒤 high-order kernel representation과 element-wise product해줌
> - Learnable CNN block은 1x1, 3x3, 1x1 convolutions로 구성됨
- 이러한 location-aware high-order kernel representation 추출 과정은 서로 다른 orders $r$에 따라 여러번 수행되며, 모든 결과가 하나의 feature maps로 concat됨
> - 이때 $r$은 1x1 convolution의 output channel dimensions와 같음
- 최종적으로 출력된 feature maps에서 RoI pooling이 진행되며, 이때 pooling operation은 average pooling이 아닌 max pooling이 사용됨

## 실험결과
- Faster R-CNN, R-FCN 등의 methods 보다 향상된 detection 성능을 기록함

## 의견
- / 