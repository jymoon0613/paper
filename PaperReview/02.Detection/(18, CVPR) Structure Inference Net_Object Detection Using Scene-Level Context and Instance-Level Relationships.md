# Structure Inference Net: Object Detection Using Scene-Level Context and Instance-Level Relationships (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Liu_Structure_Inference_Net_CVPR_2018_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/liu2018structure.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 대다수의 object detectors는 object region 주위의 local information에만 집중하는 경향이 있으며 scene context나 object relationships와 같은 contextual information은 종종 무시됨
> - Contextual information은 detection 성능에 중요함
> - 외형적으로 비슷한 objects에 대해 scene context를 고려하여 더 적합한 class로 판단할 수 있으며, 주어진 context에서 등장할 수 있는 관련된 objects의 존재를 유추할 수도 있음
- 주어진 이미지에서, scene context는 그래프 구조로 표현될 수 있음
> - 각각의 objects는 그래프의 node를 구성하며, objects 간의 관계를 edge로 표현됨
- 본 논문에서는 graph로 표현되는 objects를 모델링하기 위한 structure information network(SIN)을 제안함

## 접근법
- Faster R-CNN을 baseline으로 사용함
- RPN(+NMS) 이후 그래프 구조로 objects를 모델링함
> - Region proposals에 대해 RoI pooling과 FC layer를 적용하여 visual features를 추출함
> - 각 visual feature가 그래프의 node가 되고, 각 node 간의 관계는 edge가 됨
> - 또한 backbone의 feature maps 전체에 RoI pooling과 FC layer를 적용하여 image-level scene representation으로 사용함(visual features와 같은 dimension)
- 그래프 구조로 표현된 objects에 대해, 하나의 object는 서로 다른 objects와의 관계를 바탕으로 자신의 representation을 업데이트 해야함
> - 각 노드는 서로 다른 노드로부터 'messages'를 받아 자신의 state를 업데이트함
> - GRU를 이용함

## 실험결과
- Scene context와 object-object relationships를 이용했을 때 Faster R-CNN을 포함한 다른 methods에 비해 향상된 detection 성능 기록

## 의견
- / 