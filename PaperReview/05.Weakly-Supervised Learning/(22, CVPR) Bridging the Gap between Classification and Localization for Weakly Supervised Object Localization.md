# Bridging the Gap between Classification and Localization for Weakly Supervised Object Localization (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Kim_Bridging_the_Gap_Between_Classification_and_Localization_for_Weakly_Supervised_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/kim2022bridging.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Object localization에서 weakly-supervised한 approach는 image-level label만을 요구하기 때문에 최근 각광 받고 있음
- 특히, CAM 기반의 방법이 주로 사용됨
- 하지만 CAM 기반의 방법은 전체 object area를 identify하기 보다는 이미지의 가장 discriminative한 parts에 집중하기 때문에 localization performance가 낮다는 문제가 있음
- CAM은 크게 두 부분으로 나눌 수 있음
> - Feature map에서의 activation
> - 각 spatial location에서의 feature vector와 FC layer의 class-specific weight 간의 cosine similarity
- CAM이 전체적인 object area를 identify하지 못하는 이유는 feature와 class-specific weights 간의 잘못된 alingment 때문임
> - Feature map에서의 activation은 object의 전체적인 area가 identify 되지만, 대부분의 영역이 class-specific weights과의 cosine similarity가 낮아서 CAM에서는 나타나지 않게 됨
> - 이는 오직 target object의 가장 discriminative한 region만이 CAM에 포함되도록 함
- 이는 classification을 위한 모델이 각 spatial location에서의 피처를 고려하는 것이 아니라 모든 location의 평균적인 feature를 고려하기 때문임 (classification과 localization 사이의 gap)
- 따라서, 본 논문에서는 classification과 localization의 gap을 줄이기 위해 feature direction alignment 방법을 제안함
> - Entire object region의 feature directions을 class-specific weights difrections으로 정렬함 (동시에 background region으로의 정렬은 피함)

## 접근법
- CAM을 normalized feature map $\mathcal{F}$와 similarity map $\mathcal{S}$로 decompose할 수 있음
- $\mathcal{F}$에는 entire object area가 잘 activate되어 있지만, $\mathcal{S}$에서의 낮은 값으로 인해 최종 CAM에는 극히 일부만 나타남
- Cosine similarity는 두 벡터의 방향 사이의 alignment 정도로 해석 가능함
- Classifier는 모든 location의 평균적인 feature를 고려하여 분류하도록 학습하기 때문에 object region의 input feature vector와 class-specific weight vector가 classification 훈련 만으로는 잘 align 될 수 없다는 것을 의미함
- 전체 object 수준에서의 activation을 증진시키기 위해 target object position의 feature position과 class-weight 사이의 cosine similarity를 높여야 함
- 먼저, constant threshold를 통해 $\mathcal{F}$에서의 background region과 foreground region을 구분함
- $\mathcal{S}$에서 foreground region의 value를 높이고, background region의 value는 낮추기 위해 similarity loss를 정의함
- 마찬가지로, constant threshold를 통해 $\mathcal{S}$에서의 background region과 foreground region을 구분함
- $\mathcal{F}$에서 foreground region의 value를 높이고, background region의 value는 낮추기 위해 norm loss를 정의함
- 두 loss term의 joint optimization을 통해 $\mathcal{F}$와 $\mathcal{S}$는 유사해짐 (alignment)
- 기존의 $\mathcal{F}$에서는 덜 discriminative한 part는 작은 값을 가짐
- 따라서 $\mathcal{F}$에서 target object region의 activation을 분산시키기 위한 기법인 consistency with attentive dropout을 제안함
> - 원본 feature map $F$에서 큰 activation을 지니는 spatial region 일부를 erasing함
> - Masking된 $F$와 원본 $F$ 간의 L1 loss를 계산함
> - 이는 전체적인 object region을 참조하도록 (일부 region에 대한 의존도를 낮추도록) 하는 역할을 함

## 실험결과
- CUB, ImageNet의 localization tasks에서 SOTA 성능 달성

## 의견
- /