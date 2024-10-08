# Dive Into Ambiguity: Latent Distribution Mining and Pairwise Uncertainty Estimation for Facial Expression Recognition (CVPR, 2021)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2021/html/She_Dive_Into_Ambiguity_Latent_Distribution_Mining_and_Pairwise_Uncertainty_Estimation_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/she2021dive.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- FER의 성능을 가로막는 원인은 Annotation의 모호함에 있음 
- 이는 (1) 특정 표정이 어떤 감정을 나타내는 지는 사람마다 판단 기준이 다르고, (2) 대규모 FER 데이터셋의 경우 이미지의 Label Distribution을 제공하기 어려우며, 따라서 모호한 샘플에 의해 성능이 더는 개선되지 못함 
- 본 연구에서는 FER의 이러한 모호함 문제를 해결하기 위해 Laten Distribution Mining과 Pairwise Uncertainty Estimation이라는 두가지 관점을 사용함 (DMUE) 
- Latent Distribution Mining은 Online 방식으로 샘플의 레이블 분포를 파악하기 위해 여러 Auxiliary Branch를 사용하고, 반복적으로 업데이트되는 분포는 레이블 공간에서 주어진 이미지의 시각적 특징을 더 잘 파악할 수 있도록 함 
- Pairwise Uncertainty Estimation은 샘플 간의 관계를 기반으로 레이블 불확실성을 추정하고, 추정된 불확실 정도는 모델이 찾은 레이블 분포와 주어진 레이블 간의 학습 비중을 동적으로 조정하도록 함 

## 접근법
### (1) Overview 
- Annotation Ambiguity를 해결하기 위해 모델이 실제 이미지 레이블과 이미지의 추정 레이블 분포를 동시에 고려하도록 함 
### (2) Latent Distribution Mining 
- 주어진 이미지의 레이블 분포를 찾기 위해 클래스 개수만큼의 Branch를 생성함 
- Backbone 모델은 공유되며 상위의 Classifier만 Branch로 설계 
### (3) Pair-wise Uncertainty Estimation 
- 레이블 정확도의 불확실성을 추정하고, 이에 기반하여 신뢰도가 낮은 샘플의 Loss를 줄임 (영향력을 줄임) 

## 실험결과
- RAF-DB, AffectNet, FERPlus, SFEW 등의 데이터셋에서 검증 

## 의견
- /