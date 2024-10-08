# Cascaded Pyramid Network for Multi-Person Pose Estimation (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Chen_Cascaded_Pyramid_Network_CVPR_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/chen2018cascaded.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Multi-person pose estimation은 주어진 이미지의 모든 사람의 key points를 recognize하고 locate하는 task임
- 딥러닝의 발전과 함께 pose estimation도 큰 발전을 이루었지만, 여전히 challenging한 cases가 존재함
> - occluded keypoints, invisible keypoints and crowded background
- 이러한 문제들은 크게 (1) hard한 joints가 단순한 외형적 특징(appearance features)로는 recognize될 수 없으며, (2) hard한 joints가 학습 과정에서 명시적으로 다뤄지지 않는다는 점에서 기인함
- 본 논문에서는 이러한 hard joints 문제를 해결하기 위해 Cascaded Pyramid Network(CPN)을 제안함
- CPN은 GlobalNet과 RefineNet으로 구성됨
> - GlobalNet은 FPN을 기반으로 feature representations를 학습함
> - RefineNet은 hard joints를 처리하기 위해 사용되며, online hard keypoints mining(OHKM) loss를 사용하여 학습함
- 용어 정리
> - Single-person pose estimation: Cropped된 human bounding box를 기반으로 human keypoints를 예측함
> - Multi-person pose estimation: 이미지에 존재하는 모든 사람에 대한 keypoints를 예측함
> - Multi-person pose estimation을 수행하는 방법에는 bottom-up approach와 top-down approach가 있음
> - Bottom-up approach는 우선 이미지에 존재하는 모든 keypoints를 찾고, 이미지 속 사람들의 모습에 따라 찾은 keypoints를 적절히 조합하여 full pose를 만들어냄
> - Top-down approach는 keypoints를 찾는 과정을 two-stage로 구성함: 먼저 detector network를 사용하여 이미지에서 모든 사람의 bounding box를 crop하고, 각 bounding box에 대해 single-person pose estimation을 적용하여 keypoints를 찾음

## 접근법
- 본 논문은 top-down 방식으로 multi-person pose estimation을 수행함
- 제안하는 CPN은 GlobalNet과 RefineNet으로 구성됨
- GlobalNet은 ResNet을 backbone으로 사용함
> - 각 stage에서 feature maps를 추출한 뒤, 3x3 conv.를 적용하여 keypoints heatmaps를 생성
> - 낮은 stage의 shallow features는 high spatial resolution을 지니기 때문에 localization에 용이하지만 recognition을 위한 semantic information이 부족함
> - 반면 높은 stage의 deep features는 semantic information은 풍부하지만 low spatial resolution 때문에 정확한 localization에 불리함
> - 따라서, 이러한 semantic information과 spatial resolution의 장점을 결합하기 위해 FPN을 사용함 (기존 FPN을 살짝 변형)
> - L2 loss를 objective function으로 사용
- GlobalNet은 많은 keypoints를 정확하게 locate할 수 있음
- 하지만 GlobalNet은 주로 appearance features에 집중하고 있고 context 정보가 더 고려되어야 하는 hard keypoints에 대해서는 잘 locate하지 못함
- 따라서, GlobalNet이 생성한 feature pyramid에 대해 hard keypoints를 모델링하는 RefineNet을 추가로 정의함
> - RefineNet은 pyramid features의 resolution을 모두 맞춰주고(upsampling을 사용하여), resized된 features를 모두 결합하여 예측을 수행함
> - GlobalNet과 마찬가지로 L2 loss를 objective function으로 사용
> - 하지만, GlobalNet의 loss를 모든 N개의 keypoints에 대해서 계산되지만, RefineNet은 hard keypoints에 집중하기 위해 N개의 keypoints 중 loss가 가장 큰 M개의 keypoints만을 고려함 (OHKM loss)

## 실험결과
- MS COCO 데이터셋에서 검증함
- COCO test-challenge2017 데이터셋에서 SOTA 달성
- RefineNet의 사용과 OHKM loss의 사용이 성능 향상에 긍정적인 효과가 있었음

## 의견
- /