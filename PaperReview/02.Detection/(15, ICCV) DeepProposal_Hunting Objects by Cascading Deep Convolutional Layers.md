# DeepProposal: Hunting Objects by Cascading Deep Convolutional Layers (ICCV, 2015)

[논문링크](https://openaccess.thecvf.com/content_iccv_2015/html/Ghodrati_DeepProposal_Hunting_Objects_ICCV_2015_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/ghodrati2015deepproposal.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Object detection을 위한 대표적인 접근법으로는 two-stage cascade 방식이 있음
> - 먼저 class-independent한 object proposals를 생성함
> - 각각의 object proposal에 대해 class-specific한 분류를 수행함
- 이때 object proposals를 생성하는 데에는 두 가지의 접근법이 있음
> - Bottom-up: 원본 이미지에 대해 image segmentation, edge/contour 기반의 window generation을 사용하여 proposals를 생성
> - Top-down: 모든 가능한 object proposals에 대해 correct proposals를 식별
- 본 논문에서는 정확하고 빠른 top-down window(region, object) proposals 방법에 대해 다루고자 함 (DeepProposals)
- 특히, 해당 task를 위해 학습된 CNN의 intermediate feature maps를 활용함 
> - Feature maps를 기반으로 window proposal generation을 수행함

## 접근법
- AlexNet의 각 layer의 intermediate feature maps에서 sliding window 방식으로 region proposals를 생성하고 평가함
> - Feature maps 상에서 여러 scale, aspect ratio로 bbox를 생성하고, 각 bbox 영역에 average-pooling을 적용하여 feature를 추출한 뒤, SVM으로 objectness를 예측함
- 최상위 layer의 feature maps에서의 detection rate(recall)가 가장 좋았음
- 하지만 최상위 layer의 경우 feature maps가 너무 coarse하므로 localization 성능에서는 크게 뒤쳐졌음
> - 반면, 낮은 layer는 finer한 feature maps로 인해 localization에서 좋은 성능을 보일 수 있지만 representational power가 너무 부족함
- 본 논문에서는 이러한 특징을 상호 보완하기 위해 inverse cascade 전략을 사용함
> - 먼저, 최상위 layer의 feature maps에서 sliding window 방식으로 proposals를 generate하고, objectness scores가 큰 일부 proposals를 선택함 (NMS 적용)
> - 이후 하위 layer로 순차적으로 이동하면서 상위 layer에서 선택된 position 상에서 다시 window generation을 수행하고, 계산된 objectness score를 전달받은 상위 layer의 objectness score와 곱하여 refine함 (마찬가지로 NMS 적용)

## 실험결과
- 다른 object proposal generators 및 object detectors와 비교했을 때 향상된 성능을 보여줌

## 의견
- / 