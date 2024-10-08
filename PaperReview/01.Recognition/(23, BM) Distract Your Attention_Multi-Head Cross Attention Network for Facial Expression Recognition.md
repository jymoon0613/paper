# Distract Your Attention: Multi-Head Cross Attention Network for Facial Expression Recognition (BM, 2023)

[논문링크](https://www.mdpi.com/2313-7673/8/2/199)

<p align="center">
    <img width="600" alt='fig1' src="../img/wen2023distract.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 일반적인 이미지 분류 태스크와는 달리, 표정 클래스 간에는 공통성이 강하게 나타남 
> - 여러 표정은 본질적으로 유사한 기본 얼굴 모양을 공유하며 그 차이는 구별하기 힘들 수 있음
- 이러한 문제를 해결하기 위해 loss function에 center loss를 추가하는 방법이 제안됨
> - Center loss는 class에 속하는 샘플들을 class center와 가깝게 위치하도록 장려하여 intra-class variance를 감소시킴
> - 즉, center loss는 클래스 구분을 증진시킬 수 있음 
- 또한, attention mechanism을 통해 미세한 지역적 차이를 잡아내는 방법도 고안되었음
> - 하지만, 하나의 attention head만을 사용하는 것은 얼굴 표정의 여러 중요한 부분을 모두 잡아내지 못한다는 문제가 있음 
- 본 연구에서는 center loss의 개념을 확장시킨 feature clustering network(FCN)을 도입하여 intra-class variation 및 inter-class margins를 최적화하고자 함
- 또한, 얼굴 표정의 여러 부분에 집중하기 위한 multi-head cross attention network(MAN)와 추출된 피처의 통합을 위한 attention fusion network(AFN)를 도입하여 FER 성능을 개선하고자 함 

## 접근법
### (1) Feature Clustering Network (FCN)
- Inter-class margins을 최대화하기 위해 discriminative loss 중 하나인 affinity loss를 사용
- 학습시마다 각 클래스의 features는 클래스의 중심으로 이동하고, 서로 다른 클래스의 중심은 점점 멀어짐
- Backbone은 affinity loss를 사용하는 FCN이며, 모델은 입력 features에 대해 변형된 features를 output으로 산출함 

### (2) Multi-head Cross Attention Network (MAN) 
- 다수의 parallel cross attention heads로 구성됨 
- Cross attention head는 spatial attention unit과 channel attention unit의 조합임 
> - Spatial attention unit은 FCN에서 추출된 features에서 spatial features를 추출 
> - Channel attention unit은 추출된 spatial features에서 channel features를 추출 
- MAN을 통해 입력 표정 이미지의 다양한 지역적 피처를 추출할 수 있음 

### (3) Attention Fusion Network (AFN) 
- MAN을 통해 여러 지역적 피처를 추출했지만 그 중요도의 순서는 알지 못함 
- AFN은 log-softmax 함수를 통해 attention maps를 스케일링 함으로써 가장 집중해야 할 부분을 강조함 

## 실험결과
- AffectNet-8, AffectNet-7, RAF-DB에서 모두 SOTA 성능 기록 

## 의견
- / 