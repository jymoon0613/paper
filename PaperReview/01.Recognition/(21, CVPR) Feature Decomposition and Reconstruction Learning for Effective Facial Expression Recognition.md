# Feature Decomposition and Reconstruction Learning for Effective Facial Expression Recognition (CVPR, 2021)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2021/html/Ruan_Feature_Decomposition_and_Reconstruction_Learning_for_Effective_Facial_Expression_Recognition_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/ruan2021feature.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- FER 태스크는 클래스 간 유사도가 높아 구분이 쉽지 않고 어려움 
- 표정 정보는 모든 표정에 공통으로 포함되는 정보 (Expression Similarities)와 특정 표정만의 고유한 정보 (Expression-specific Variations)로 구성되어 있음 
- Expression Similarities는 여러 표정 간에 공유되는 latent Features로 표현될 수 있으며, Expression-specific Variations는 Latent Features에 대한 중요도 가중치로 표현될 수 있음 
- 따라서 어떠한 특정 표정의 Expression Features는 중요도 가중치가 적용된 Latent Features로 표현될 수 있음 
- 이러한 점을 고려하여, 본 연구에서는 Feature Decomposition and Reconstruction Learning (FDRL)을 제안하여 FER을 수행하고자 함 

## 접근법
### (1) Overview 
- 제안된 네트워크는 Feature Decomposition Network (FDN), Feature Reconstruction Network (FRN), Expression Prediction Network (EPN)으로 이루어짐 
- Feature Reconstruction Network는 Intra-feature Relation Modeling Module (Intra-RM)과 Inter-feature Relation Modeling Module (Inter-RM)으로 이루어짐 
### (2) Feature Decomposition Network (FDN) 
- Expression Similarities를 추출하는 네트워크 
- 입력 이미지는 Backbone Network (ResNet18)을 거쳐 하나의 벡터로 출력됨 
- 각 이미지는 사전에 정의한 M 개의 Latent Feature로 변환됨 
- 모든 이미지의 Latent Feature는 유사해질 필요가 있음 (모든 클래스에 공통적인 특징을 찾는 과정이므로) 
- 따라서, Center Loss와 유사한 Compactness Loss를 정의하여 M 개의 Latent Feature가 모든 이미지 간에 유사하도록 유도함 
### (3) Feature Reconstruction Network (FRN) 
- Expression-specific Variations를 추출하는 네트워크이며 Intra-RM과 Inter-RM으로 구성됨 
- Intra-RM에서는 각 이미지 별로 M개의 Latent Feature의 중요도 가중치를 도출 
- Balance Loss와 Distribution Loss를 사용하여 같은 클래스에 속하는 이미지들은 Latent Feature에 대한 중요도 가중치가 유사해지도록 학습 
- Inter-RM에서는 Graph Neural Network 구조를 사용하여 각 Latent Feature의 간의 관계를 파악함 

## 실험결과
- In-the-lab Database (CK+, MMI, Oulu-CASIA) 및 In-the-wild Database (RAF-DB, SFEW) 모두에서 평가함 
- Backbone 모델인 ResNet18은 Face Recognition Datasets로 사전 훈련되어 있음 

## 의견
- 여러 Loss 함수를 사용하여 학습 (복잡함) 