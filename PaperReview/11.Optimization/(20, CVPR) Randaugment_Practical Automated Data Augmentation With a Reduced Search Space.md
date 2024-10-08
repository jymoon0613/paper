# Randaugment: Practical Automated Data Augmentation With a Reduced Search Space (CVPR, 2020)

[논문링크](https://openaccess.thecvf.com/content_CVPRW_2020/html/w40/Cubuk_Randaugment_Practical_Automated_Data_Augmentation_With_a_Reduced_Search_Space_CVPRW_2020_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/cubuk2020randaugment.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 기존 data augmentations은 manually designed되어 새로운 task에 대해서는 확장되지 못한다는 단점이 있었음
- 주어진 데이터셋에 대한 최적의 augmentations를 자동으로 찾는 automated augmentation은 여러 vision tasks에서 성능 향상 효과를 보여줌
- 하지만 기존의 automated augmentation은 search 과정이 개별 augmentation에 대해 각각 수행되기 때문에 expensive하고 모델과 데이터셋 크기에 민감하다는 문제가 있음
- 본 논문에서는 search space 단순화를 통해 automated augmentation의 cost를 줄이고 훨씬 실용적인 효과를 가지도록 함 (RandAugment)

## 접근법
- Parameter를 줄이기 위해 14개의 augmentations가 uniform probability로 선택됨
- RandAugment는 두 개의 parameter $N$과 $M$으로 구성됨
> - 하나의 training image에 $N$개의 transformations가 $M$의 distortion strength로 적용됨
> - $N$과 $M$이 커질수록 훨씬 강력한 augmentation이 됨
- 설정한 $N$과 $M$을 바탕으로 최적의 augmentations를 찾기 위한 grid search 진행

## 실험결과
- RandAugment는 search cost가 매우 작음에도 불구하고 다른 automated augmentation와 비슷하거나 더 좋은 성능을 보여줌

## 의견
- /