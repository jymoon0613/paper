# YOLOv3: An Incremental Improvement (arXiv, 2018)

[논문링크](https://arxiv.org/abs/1804.02767)

<p align="center">
    <img width="400" alt='fig1' src="../img/redmon2018yolov3.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- YOLOv2에 당시 최신 기법들을 적용하여 성능 및 속도 개선

## 접근법
- Bounding box prediction은 YOLOv2와 동일
- Class 예측에는 softmax classifier가 아닌 class별 logistic classifier를 사용함
> - Complex datasets에서는 overlapping labels가 존재 ('Woman' and 'Person')
> - 이러한 경우 softmax classifier는 정확히 하나의 class에만 해당한다고 가정하므로 성능 향상에 문제가 있음
> - Logistic classifier를 사용한 multilabel 접근법이 더 적합
- FPN과 마찬가지로 3가지 다른 scales에서 box를 예측함
- K-means clustering 사용하여 anchor boxes 생성
- Darknet-19 backbone을 사용하며 추가로 ResNet의 shorcut connection 적용

## 실험결과
- 전체적인 AP로 평가했을 때는 성능이 낮았지만
- IOU가 0.5일 때의 AP로 평가했을 때는 성능이 좋았음

## 의견
- /