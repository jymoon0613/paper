# Spatially Consistent Representation Learning (CVPR, 2021)

[논문링크](https://openaccess.thecvf.com/content/CVPR2021/html/Roh_Spatially_Consistent_Representation_Learning_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/roh2021spatially.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Unsupervised representation learning은 여러 downstream tasks에서의 성능 향상을 증진시켰음
- 여러 방법 중 image contrastive 기반의 방법이 성능이 뛰어났음
- 하지만 기존의 contrastive methods는 이미지 단위의 global representation을 기반으로 학습하기 때문에 local region에 대한 representation 학습 기능이 부족함
> - 만약, 한 이미지 내에 존재하는 특정 object가 기하학적으로 이동하거나 크기가 조정되었을 때 기존의 contrastive methods는 local region의 변화를 고려하지 않고 유사한 표현을 생성하게 됨
> - 두 개의 augmented views에 대해 local object의 변화와는 상관없이 두 이미지의 representation이 유사해지도록 학습 (global consistency)
- 이러한 단점은 spatial representations에 기반한 localization tasks에서의 contrastive learning의 성능을 저하시키는 원인이 됨
- 또한, 기존의 contrastive methods는 positive pairs를 생성하기 위해 heavy하게 cropped된 이미지를 사용하지만, 이는 오히려 의미론적으로 상관이 없는 image region간의 positive 매칭을 유도할 수 있음
- 따라서, 본 논문에서는 이러한 global한 contrastive learning methods의 문제를 해결하기 위해 spatially consistent한 representation learning method인 SCRL을 제안함
> - 이미지 차원의 global consistency가 아닌 이미지의 local object 차원의 representation이 유사해지도록 학습 (spatial/local consistency)
- SCRL은 detection/segmentation을 포함한 location-specific하고 multi-object한 downstream tasks에 특히 강점이 있음

## 접근법
- 전체 아키텍처는 BYOL의 아키텍처를 사용함 (siamese networks, momentum update)
- 하나의 이미지에 random augmentation을 적용하여 두 views를 생성함
- 두 views는 global average pooling layer 이전으로 절단된 online 및 target network를 통과하여 spatial feature maps로 encoding됨
- Global representation의 distance를 최소화했던 이전의 방법들과 달리 SCRL은 같은 sptial regions 및 semantic meaning을 갖는 local regions의 representation distance를 최소화함
- 이를 위해, 우선 원본 이미지에서 두 augmented views의 intersection regions를 찾음 (두 views가 공통적으로 포함하고 있는 원본 이미지에서의 이미지 영역)
- 찾은 intersection regions에서 임의의 bounding box를 랜덤하게 샘플링함 (10개)
> - box들의 diversity 증가를 위해 샘플링된 두 박스 간의 IoU가 50%가 넘으면 reject
- 샘플링한 box의 좌표를 두 views의 공간 상의 좌표로 변환함 ($B_1, B_2$)
- $B_1, B_2$는 hard augmentation에 의해 size, location, internal color 등의 측면에서 상이하지만 같은 semantic meaning을 내포한다는 점은 변하지 않음
- 이때, $B_1, B_2$를 서로 정확하게 매핑하기 위해 shear operations 및 rotations과 같은 affine transformations(augmentation)은 사용하지 않음
- 생성된 $B_1, B_2$를 각각 online 및 target CNN에 전달하고 Mask RCNN의 RoI align을 적용하여 고정된 크기의 feature map을 생성
- 이후 projection과 prediction을 거쳐 각 representation간 similarity loss 계산 (두 views의 위치를 swap하여 총 2번 계산 가능, BYOL과 동일)

## 실험결과
- Localization tasks에서 supervised baseline 및 다른 self-supervised methods를 능가 (PASCAL VOC detection, COCO detection, COCO segmentation, etc.)

## 의견
- /