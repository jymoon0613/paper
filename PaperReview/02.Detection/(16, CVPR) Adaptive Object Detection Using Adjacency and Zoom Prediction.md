# Adaptive Object Detection Using Adjacency and Zoom Prediction (CVPR, 2016)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Lu_Adaptive_Object_Detection_CVPR_2016_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/lu2016adaptive.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 일정한 anchor boxes의 집합을 사용하는 이전의 region proposal 방식 대신 adaptive한 탐색 전략을 사용하고자 함
- 제안하는 방법은 전체 이미지를 반복적으로 sub-regions로 분할하며, 해당 과정을 한 region에 하나의 object만 포함된다고 판단될 때까지 반복함
> - 분할 과정에서 생성되는 모든 sub-regions를 anchor boxes로 사용함
- 생성된 sub-regions에 대해 어떤 sub-region을 더 분할할지에 대한 결정은 각 sub-region의 추출된 feature saliency에 의해 결정됨
> - 즉, anchor boxes의 생성이 image content에 의해 좌우됨 (adaptive함)
> - 소수의 small objects만 존재하는 이미지의 경우 결과적으로 objects 주변의 small anchor regions가 집중적으로 생성되게 됨
> - 반대로 large objects만 존재하는 이미지의 경우 결과적으로 large object에 대한 large anchor regions가 집중적으로 생성됨
> - 이러한 과정은 불필요한 anchor boxes에 대한 고려를 줄여서 computation도 절약할 수 있음
- 이러한 작업을 수행하는 네트워크인 adjacency and zoom network(AZ-Net)을 설계함

## 접근법
- 본 연구에서 제안하는 detection 알고리즘은 two-step으로 구성됨
> - (i) AZ-Net을 사용하여 class-independent한 region proposals를 생성함
> - (ii) 각 region proposal에 대해 Fast R-CNN detector가 class-wise detection을 수행함
> - 본 연구는 (i)에 집중함
- (i)에서는 반복적인 search 전략이 사용됨
> - 전체 이미지를 root region으로 사용하여 반복적으로 region을 sub-regions로 분할함
> - Sub-regions는 주어진 region을 정확히 4등분한 영역과, 정중앙 영역의 5개로 구성됨
> - 이때 생성된 sub-regions에 대해 features를 추출하고 zoom indicator와 adjacency predictions를 계산함
> - Adjacency predictions가 특정 threshold를 넘는 sub-regions는 output region proposals에 포함시킴
> - Zoom indicator가 특정 threshold를 넘는 sub-regions는 더 작은 objects를 포함하고 있을 가능성이 크다는 뜻이므로 다시 sub-regions로의 분할을 수행함
> - 반복은 모든 가능한 sub-regions의 zoom indicator가 threshold보다 작은 경우에 중단함
- AZ-Net은 VGG-16 기반의 Fast R-CNN이지만 search 전략에 맞게 훈련 방식을 일부 수정함
> - 먼저, 입력 이미지로부터 regions가 샘플링됨
> - 샘플링된 regions는 zoom indicator와 adjacency prediction을 위한 hard positive/negative examples를 포함하고 있음
> - 각 region에 대해 AZ-Net은 zoom indicator와 adjacency prediction을 예측함
> - 이때 zoom indicator와 adjacency prediction은 훈련 task에 맞게 적절히 labelling됨

## 실험결과
- Fast R-CNN 및 Faster R-CNN보다 적은 region proposals로 좋은 성능을 달성함

## 의견
- / 