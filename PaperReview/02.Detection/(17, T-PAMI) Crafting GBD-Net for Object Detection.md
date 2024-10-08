# Crafting GBD-Net for Object Detection (T-PAMI, 2017)

[논문링크](https://ieeexplore.ieee.org/abstract/document/8017422)

<p align="center">
    <img width="600" alt='fig1' src="../img/zeng2017crafting.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Detection 성능을 향상시키기 위해 object 주변의 contextual information을 사용하도록 함
> - Background, nearby objects
- 본 논문에서는 object와 관련된 contextual, local visual cues의 interactions를 adaptive하게 모델링하는 gated bi-directional CNN(GBD-Net)을 제안함

## 접근법
- Faster R-CNN 프레임워크를 사용함
- Faster R-CNN의 RoI pooling 단계에서 candidate box의 중심을 기준으로 서로 다른 size의 regions가 pooled됨
> - RoI pooling 이후에는 모두 동일한 사이즈의 features로 추출됨
> - Pooled features는 서로 다른 resolution의 support regions를 의미함
- Pooled features가 gated bi-directional(GBD) network로 입력됨
- GBD network에서 각 pooled features는 higher resolution(small-context), lower resolution(large-context)의 features를 모두 사용하여 업데이트됨 (bi-directional)
> - Bi-FPN과 유사한 컨셉
- 또한, 양방향 정보를 전달받을 때 gated function을 통해 정보의 흐름을 조절함
> - Adaptive하게 contextual, local visual cues의 interactions를 조절
> - Convolution(w/ sigmoid)으로 구현
- GBD network의 모든 outputs가 detection branch로 전달되어 detection 수행

## 실험결과
- ImageNet Object Detection, PASCAL VOC2007, MS COCO에서 모두 좋은 성능 달성

## 의견
- / 