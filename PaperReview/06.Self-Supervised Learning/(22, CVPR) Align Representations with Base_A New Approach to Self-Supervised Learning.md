# Align Representations with Base: A New Approach to Self-Supervised Learning (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Zhang_Align_Representations_With_Base_A_New_Approach_to_Self-Supervised_Learning_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2022align.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Labeled data의 희소성으로 인해 unsupervised 및 self-supervised learning이 주목받고 있음
- Generative 및 discriminative(pretext, contrastive)한 self-supervised 방법들이 있으며 그 중에서도 최근에는 contrastive learning이 주를 이룸
- Contrastive learning의 핵심은 random augmented된 두 views가 embedding 수준에서 서로 유사한 semantic information을 지니도록 하는 것
- 하지만 embedding 수준에서 두 views를 바로 align하는 것은 모델의 붕괴를 유발함
- 이를 해결하기 위해 SimCLR은 negative samples를 이용하였지만 연산량이 매우 많아진다는 문제가 있었음
- BYOL과 SimSiam은 아키텍처 조정을 통해 negative samples를 사용하지 않아도 모델이 붕괴되지 않도록 했지만, 어떻게 모델이 안정적으로 수렴할 수 있는지에 대한 이론적 증명이 부족함
- 따라서, 본 논문에서는 Align Representations with Base (ARB)라는 새로운 self-supervised 방법을 제안함
- ARB는 contrastive learning의 일종으로서 모델 붕괴를 유발하지 않으면서도 간단명료하고, 이해하기 쉬우며, 효율적임 (negative samples 사용 X)

## 접근법
- ARB는 negative samples를 사용하지 않지만 asymmetric 구조(e.g. BYOL)가 아닌 symmetric한 구조를 사용함
- 입력 이미지는 random augmentation에 의해 두 views로 변환됨
- 두 views는 shared encoder(backbone) 및 projector(MLP)를 거치면서 처리됨
- 결과로 얻은 두 views의 representations에 대해 nearest orthonormal bases를 계산함
- 최종 목적함수는 한 view의 representation과 또다른 view의 orthonormal base간의 유사도가 최대화되록 함 (Aligning Representation with Base)

## 실험결과
- Base에 align 시키는 것은 representation의 entropy와 variance를 증가시키므로 모델 붕괴를 방지할 수 있음
- 또한 ARB는 projector의 output dimension 크기에 대해 강건함 (vs. Barlow Twins)

## 의견
- /