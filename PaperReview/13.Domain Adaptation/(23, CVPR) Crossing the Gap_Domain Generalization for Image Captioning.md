# Crossing the Gap: Domain Generalization for Image Captioning (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Ren_Crossing_the_Gap_Domain_Generalization_for_Image_Captioning_CVPR_2023_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/ren2023crossing.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 image captioning task에서의 doamin generalization을 다룸(DGIC)
- 기존 image captioning 기법들은 domain-specific한 features에 overfit되는 경향이 있고, 기존 domain generalization 기법들은 대부분 간단한 tasks에서 검증되어 복잡한 image captioning task에 바로 적용하기에는 어려움이 있음
- 따라서, DGIC task를 위한 language-guided semantic metric learning(LSML) 프레임워크를 제안함

## 접근법
- Domain-independent한 visual components를 학습하기 위해 multiple source domains에 대한 inter-/intra-domain metric learning을 적용함
> - Anchor image features를 선정하고, anchor features와의 feature similarity에 따라 선정된 positive samples(features)와 negative samples(features)에 대해 triplet loss를 적용
- 이때 positive/negative samples는 sentence similarity, visual word similarity를 통해 정의됨(semantic text guidance)

## 실험결과
- DGIC 환경에서 다른 domain generalization 기법보다 좋은 성능 달성

## 의견
- /