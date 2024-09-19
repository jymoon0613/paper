# Region Attention Networks for Pose and Occlusion Robust Facial Expression Recognition (IEEE T-IP, 2020)

[논문 링크](https://ieeexplore.ieee.org/abstract/document/8974606)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2020region.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Occlusion과 Variant Pose는 FER에서 중요한 문제임 
- 최근의 FER 연구는 많이 제시되었지만 Occlusion과 Variant Pose에 관한 문제는 다루는 경우가 적음 (주로 관련된 Annotation이 있는 데이터셋이 부족하기 때문) 
- 명시적으로 Occlusion을 찾고 지우는 것은 Occlusion 상황이 매우 다양하고 Detect가 어렵다는 실세계의 상황과 동떨어지고, 전체 이미지에 대해 CNN을 바로 적용하는 것은 Occlusion과 Variant Pose를 세밀하게 다루기에 적합하지 않음 
- 사람은 불완전한 표정 정보가 주어지더라도 전체적인 얼굴 정보와 지역적인 얼굴 정보를 같이 활용하여 FER을 할 수 있음 
- 따라서, 본 연구에서는 이러한 점에 착안하여 입력 이미지의 전체적인 정보와 지역적인 정보를 같이 사용하여 Occlusion과 Variant Pose에 강건한 FER 모델을 제안하고자 함 
- 또한, AffectNet, FER+, RAF-DB 데이터셋에 Occlusion과 Variant Pose를 추가한 데이터셋을 생성함 

## 접근법
### (1) Overview 
- 불필요한 Occlusion과 Irrelated Regions를 제거하되, End-to-End Deep Architecture로 자동화함 
- 제안하는 Region Attention Network (RAN)은 Region Cropping & Feature Extraction Module, Self-attention Module, Relation-attention Module로 구성됨 
### (2) Region Cropping & Feature Extraction Module 
- 얼굴 이미지를 여러 Regions으로 Crop 하고, 각 Cropped 이미지는 Backbone Network를 거쳐 Region Feature Extraction을 수행함 
### (3) Self-attention Module 
- Backbone으로부터 추출한 피처 벡터에, FC Layers를 거쳐 생성된 Attention Maps를 곱하여 Self-attention을 수행 
- 결과는 하나의 Vector로 Aggregate 됨 
- Self-attention의 효과를 최대화하기 위해 Region Biased Loss (RB-loss) 사용 
- 각 표정 클래스마다 서로 다른 얼굴 영역이 중요하다는 점에서 착안하여 Crop된 이미지의 Attention 가중치가 원본 얼굴 이미지의 가중치보다 커야 한다는 것을 강제함 
- 제안된 RB-loss는 Region Attention의 효과를 향상시키고 RAN이 지역 및 전역 표현의 좋은 가중치를 학습하도록 권장할 수 있음 
### (4) Region-attention Module 
- 전역적인 Global Attention을 수행하기 위해 Aggregated된 Self-attention Output과 각 Region Feature와의 Attention을 수행하고 결과를 Aggregate하여 최종적인 Feature를 추출함 
- Classification Layer를 거쳐 최종적인 분류 수행 

## 실험결과
- 여러 FER 데이터셋에서 평가함 
- Occlusion과 Pose Variant가 추가된 데이터셋도 사용 
- Backbone 모델은 Face Detection 데이터셋으로 사전 훈련되어 있음 

## 의견
- Occlusion과 Variant Pose에 특화되어 있음 