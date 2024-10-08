# Deep Hierarchical Semantic Segmentation (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Li_Deep_Hierarchical_Semantic_Segmentation_CVPR_2022_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/li2022deep.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근의 거의 모든 semantic segmentation methods는 단순히 이미지 내에서 모든 tager classes가 분리되어 있다고 가정하며 pixel-wise prediction만을 고려하고 있음
- 이러한 접근법은 복잡한 scene이 단순한 entities의 조합으로 구성된다는 visual world의 hierarchical한 특성을 반영하지 못함
> - e.g., Vehicle - Bus, Truck, Motocycle - Wheel, Window, etc.
- 이러한 hierarchical decomposition은 여러 vision task에서 다루어졌지만 semantic segmentation에서는 거의 탐구되지 않음
- 따라서, 본 논문에서는 hierarchical semantic segmentation(HSS)을 위한 새로운 접근법을 제안함 (HSSN)
- HSSN은 우선 pixel-wise hierarchical segmentation learning 전략을 사용함
> - HSSN에서 class label은 root-to-leaf path의 class hierarchy로 구성됨 (i.e., human -> rider -> bicyclist)
> - 이를 통해 HSSN은 기존의 semantic segmentation task를 pixel-wise multi-label classification task로 간단히 재정의함으로써 기존의 semantic segmentation model들을 쉽게 HSS 환경에 활용할 수 있도록 함
> - 또한, HSSN은 HSS 학습 시 segmentation predictions이 class hierarchy를 완전히 반영하도록 함
> - 이를 위해 (i) 특정 class에 속하는 pixel sample은 항상 해당 class의 상위 class에도 속해야하며, (ii) 특정 class에 속하지 않는 pixel sample은 항상 해당 class의 하위 class에 속하지 않아야 한다는 두 가지 제약을 optimization에 추가함
- 또한, HSSN은 pixel-wise hierarchical representation learning 전략을 사용함
> - Class hierarchy를 pixel embedding space에도 반영
> - 의미론적으로 유사한 pixels의 embedding은 가까워지도록 하고(i.e., bicycle and motocycle), 의미론적으로 유사하지 않는 pixels의 embedding은 멀어지도록 함(i.e., pedestrian and lamppost)

## 접근법
- 기존의 semantic segmentation networks와 동일하게 feature encoder와 segmentation head로 구성된 아키텍처를 사용함
- 이때 segmentation head는 HSS setting에 맞게 전체 class hierarchy를 예측하도록 수정됨
> - Root-to-leaf path를 구성하는 모든 개별 class를 binary로 예측
> - Binary cross-entropy(BCE) loss로 최적화
- Inference 시에는 각 픽셀이 prediction score가 가장 높은 root-to-leaf path로 할당됨
> - 이러한 inference 방식은 pixel-wise prediction과 class hierarchy를 같이 고려할 수 있게 해줌
- 하지만 training 시 binary cross-entropy는 개별 class에 대해 독립적으로 계산되므로 class 간의 hierarchical relation이 반영되지 못함
- 이러한 문제를 해결하기 위해 hierarchyaware segmentation learning 전략을 사용함
- Hierarchyaware segmentation learning 전략은 pixel-wise hierarchical segmentation learning 전략과 pixel-wise hierarchical representation learning전략으로 구성됨
### (1) Pixel-wise Hierarchical Segmentation Learning
- 각 픽셀에 대해, 어떤 한 class가 positive일 때 해당 class의 모든 상위 class에 대해 positive로 할당되고, 어떤 한 class가 negative일 때 해당 class의 모든 하위 class에 대해 negative로 할당되면 해당 label은 hierarchically consistent하다고 정의함
- 이러한 hierarchically consistent를 HSSN training에 접목시키기 위해 hierarchy constraints을 도입함
> - 각 픽셀에 대해, 어떤 한 class가 positive일 때 해당 class의 모든 상위 class의 prediction score는 해당 class의 prediction score보다 항상 크거나 같아야 하며, 어떤 한 class가 negative일 때 해당 class의 모든 하위 class의 prediction score는 해당 class의 prediction score보다 항상 작거나 같아야 함
- 이러한 두 constraints를 반영한 loss function인 Tree-Min Loss를 정의하여 BCE loss를 대체함
> - 예측이 어려운 pixels에 집중하도록 focal loss의 개념도 추가함 (Focal Tree-Min Loss)

### (2) Pixel-wise Hierarchical Representation Learning
- Tree-triplet loss 사용
> - Anchor, positive, negative samples가 사용됨 (각각은 batch에서 샘플링됨)
> - Positive samples는 negative samples보다 anchor pixels와 의미론적으로 더 유사함
> - Positive samples와 anchor pixels의 embedding 거리는 가까워지도록, negative samples와 anchor pixels의 embedding 거리는 멀어지도록 loss를 구성
> - Distance(dissimilarity) measure로는 cosine distance를 사용
  
## 실험결과
- 기존의 semantic segmentation models(hierarchy-agnostic segmentation models)에 쉽게 결합될 수 있음
- 여러 데이터셋에서 SOTA 성능 달성

## 의견
- /