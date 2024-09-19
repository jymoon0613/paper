# Facial Expression and Attributes Recognition based on Multi-task Learning of Lightweight Neural Networks (SISY, 2021)

[논문 링크](https://ieeexplore.ieee.org/abstract/document/9582508)

<p align="center">
    <img width="400" alt='fig1' src="../img/savchenko2021facial.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 기존의 FER 수행 모델들은 방대한 모델 사이즈를 지니는 경우가 많으며, 새로운 태스크를 수행할 때마다 많은 양의 데이터로 튜닝해야 함 
- 따라서 Mobile Device 혹은 Edge Device에서도 사용 가능한 가벼운 FER 모델이 필요함 
- 본 연구에서는 FER을 포함한 여러 Facial Analysis를 수행할 수 있는 모델을 개발하되, 가벼운 네트워크 구조를 사용하고 훈련 과정을 간소화하여 가볍지만 정확도가 높은 모델을 제안하고자 함 

## 접근법
- MobileNet, EfficientNet, RexNet과 같은 가벼운 CNN 모델을 Backbone Network로 사용 
- VGGFace2라는 거대한 Face Recognition 데이터셋으로 Backbone Network를 Pre-training 함 
- 사전 훈련된 네트워크는 입력 이미지로부터 피처를 뽑아내고, 추출된 피처는 Age Prediction, Gender Prediction, Ethnicity Prediction, Emotion Recognition 등의 과제에 사용됨 
- 훈련 과정 단순화를 위해 한 가지 태스크에서 시작하여 점차 수행되는 태스크의 개수를 늘림 
Gender → Age → Ethnicity → Emotion (Sequential Training) 

## 실험결과
- Face Identification에서 준수한 성능을 보임 
- AffectNet 데이터셋을 통해 FER의 성능을 확인 
> - 8개 클래스에 대해 학습하는 것이 더 나음 (훈련된 모델은 8개 또는 7개의 감정 예측 문제 모두에 활용 가능하므로) (더욱 보편적) 
> - ImageNet에서 사전 훈련된 모델보다 제안된 방법으로 훈련된 모델의 성능이 더 좋았음 
> - 여백을 제거하여 Compact한 이미지로 훈련한 경우 성능이 더 좋았음 
> - AffectNet 데이터셋에서 SOTA 성능 기록 
- AffectNet 데이터셋에서 SOTA 성능 기록 

## 의견
- Multitask Learning의 단점? Class Label 너무 많이 필요 (나이, 인종, …) 
- 추가적인 정보 및 레이블을 위해 많은 데이터 필요, 많은 제약 존재 
- Multitask Learning도 결국 Representation을 다각면에서 학습하는 것이라면 이에 더 적합한 Self-supervised Learning을 사용하는 것이 합리적 