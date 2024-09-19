# Feature Pyramid Networks for Object Detection (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Lin_Feature_Pyramid_Networks_CVPR_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/lin2017feature.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 크기가 매우 다른 object를 인식하는 것은 computer vision에서 매우 어려운 과제임
- 이러한 문제를 해결하기 위해 manually engineered features가 널리 사용되던 시절에는 서로 다른 스케일의 이미지를 각각 처리하여 에측하도록 하는 방식이 사용되었음 (Featurized Image Pyramid, 그림 (a))
- 이미지의 스케일이 변함에 따라 object의 서로 다른 scale을 탐구할 수 있으므로 해당 방법은 scale-invarint함
- 하지만 자체적으로 scale-invariant한 특성을 지니는 CNN의 등장으로 이러한 방식은 점점 대체됨 (Single Feature Map, 그림 (b))
- CNN은 내부적인 여러 순차적인 레이어를 거치면서 feature hierarchy를 계산함
- 하지만 CNN으로 좋은 detection 성능을 내기 위해서는 여전히 featurized image pyramid 방식이 필요했음
- Featurized image pyramid이 여전히 중요한 이유는 semantically strong한 multi-scale feature representation을 생성할 수 있다는 점
- 하지만 여러 스케일의 이미지를 처리해야 한다는 점에서 실제 활용하기에는 계산비용이 매우 큼
- CNN의 in-network feature hierarchy를 활용하는 것이 좋아보이지만, 서로 다른 level의 피처맵 간에 큰 semantic gap이 존재한다는 단점이 명확했음 (resolution이 큰 초기 레이어의 피처맵은 low-level features 정보를 담고 있고, resolution이 작은 후기의 레이어의 피처맵은 high-level features 정보를 담고 있음)
- Single Shot Detector (SSD)는 이러한 CNN의 in-network feature hierarchy를 활용하기 위해 각 레이어의 output feature map을 각각 예측에 사용함 (Pyramidal feature hierarchy, 그림 (c))
- 하지만 SSD는 small object detection에 중요한 low-level 피처맵을 사용하지 않음
- 본 연구의 목적은 CNN의 in-network feature hierarchy을 최대한 활용하되 featurized image pyramid의 semantically strong한 multi-scale feature representation이라는 특징을 지니도록 하는 것
- 이를 위해 low-resolution 및 high-resolution 피처맵을 top-down 방식 및 lateral connection 방식으로 결합함 (Feature Pyramid Network (FPN), 그림 (d))

## 접근법
- Backbone으로부터 총 4가지 스케일의 피처맵을 추출함
- High-level 피처맵은 매우 semantic하지만 resolution이 작고, low-level 피처맵은 resolution이 크지만 semantic 정보가 매우 적음
- 따라서, resolution의 이점이 있는 low-level 피처맵을 semantically strong하게 만들기 위해 high-level 피처맵으로부터 순차적으로 semantic 정보를 이식
- 먼저 최상위 피처맵은 resolution이 2배로 upsample되며, 상위 피처맵과 하위 피처맵은 모두 1x1 convolution으로 채널이 조정되며, point-wise add로 연결됨
- 최상위 피처맵을 제외한 later conntection을 거친 output은 upsample로 인한 aliasing 제거를 위해 3x3 convlution을 추가로 처리
- 제안하는 FPN 구조는 Region Proposal Network (RPN), Faster R-CNN 같은 기존 object detectore에 쉽게 적용 가능함

## 실험결과
- COCO detection set으로 object detection 성능 평가
- FPN 구조는 RPN의 bounding box proposal results를 크게 개선함
- 특히 small object에 대한 proposal 성능이 크게 개선됨
- Object detection의 경우도 성능 개선이 있었으며 FPN을 사용한 Faster R-CNN은 SOTA 성능을 뛰어넘음
- FPN은 general한 구조로서 여러 태스크에 적용 가능하며 이를 증명하기 위해 semantic segmentation에서의 성능 개선도 보여줌

## 의견
- 실험 매우 구체적
- 표 디자인 방식 매우 참고
- 서론에서 이전의 방법들과 제안하는 방법이 어떻게 다른지 비교하여 설명하는 부분 매우 좋음
