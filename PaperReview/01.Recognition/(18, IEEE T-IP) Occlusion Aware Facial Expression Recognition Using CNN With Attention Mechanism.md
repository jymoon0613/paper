# Occlusion Aware Facial Expression Recognition Using CNN With Attention Mechanism (IEEE T-IP, 2018)

[논문 링크](https://ieeexplore.ieee.org/abstract/document/8576656)

<p align="center">
    <img width="600" alt='fig1' src="../img/li2018occlusion.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 기존의 FER 모델들은 잘 정돈된 데이터셋에서 훈련됨 (통제된 환경에서 수집) 
- 하지만, 이러한 경우 실제 세계의 데이터에서 잘 동작하지 못할 가능성이 큼 (통제되지 않은 상태, 자연 상태의 이미지) 
- 특히, 얼굴의 일부분이 가려져 있는 경우 (Occlusion) FER은 매우 어려워질 수 있음 
- Occlusion은 가리는 물체가 무엇인지, 어디를 가리고 있는지에 따라 매우 상이하므로 미리 예측하여 제거할 수 없음 
- 따라서, 본 연구를 통해 통제되지 않는 상태의 실제 세계의 데이터에서도 잘 동작하는, 특히, Occlusion 상황에서도 FER을 잘 수행할 수 있는 Convolution Neural Network with Attention Mechanism (ACNN) 모델을 제안하고자 함 
- 인간은 FER을 수행할 때 얼굴을 여러 패치로 구분하고 각각의 패치를 분석하여 판단함
- 이때, 어느 패치가 가려져 있어 판단이 어려우면, 가려진 패치와 Symmetric한 패치를 참고하거나, 가장 관련이 깊은 패치를 참고하는 방식으로 판단을 내림 
- 따라서, 제안하는 ACNN 모델도 자동적으로 얼굴의 가려진 부분을 인식하고 가려지지 않은 얼굴 패치에 더욱 집중함으로써 인간의 방식과 유사하게 FER을 수행함 

## 접근법
### (1) Overview 
- 입력 얼굴 이미지는 Backbone 네트워크를 통과하여 Feature Maps를 추출 
- 이후, ACNN은 추출된 Feature Maps를 여러 개의 지역적 패치들로 분해함 
- 각각의 지역적 패치들은 Patch-Gated Unit (PG-Unit)을 거쳐 가려진 정도 (or 중요도)에 따라 가중된 벡터로 인코딩 됨 
- 또한, 지역적 패치가 아닌 전체 이미지에 대한 Feature Maps이 Global-gated Unit (GG-Unit)을 거쳐 가려진 정도 (or 중요도)에 따라 가중된 벡터로 인코딩 됨 
- 추출된 지역적 패치 벡터들과 전체 이미지 벡터는 합쳐져서 입력 이미지에 대한 표현으로서 기능함 
- 추출된 이미지 표현을 두 개의 FC Layer를 거쳐 FER을 수행함 
- PG-Unit만 사용하는 네트워크는 pACNN, GG-Unit도 사용하는 네트워크는 gACNN으로 구분 
### (2) Patch Based ACNN (pACNN) 
- FER을 수행하기 위해서는 얼굴 근육의 지역적 움직임을 잡아낼 수 있어야 함 
- 이를 위해, pACNN은 얼굴 이미지를 여러 개의 패치들로 구분하고, Gate Unit을 통해 가려짐 (or 중요도)의 정도를 스케일링 함 
- PG-Unit에서 각 패치들은 벡터로 인코딩 되며, Attention Module을 통해 내부적으로 계산된 가중치로 스케일링 됨 
### (3) Global-local Based ACNN (gACNN) 
- pACNN은 패치 단위의 지역적 특징을 학습하지만 이미지 전체의 전역적인 특징은 고려하지 못하고 있음 
- 따라서, gACNN은 GG-Unit을 사용하여 지역적 특징과 전역적 특징을 같이 사용 
- 벡터 인코딩과 가중치 계산은 pACNN과 유사하게 진행됨 

## 실험결과
- 일반적인 FER 데이터셋 (통제된 환경에서 수집) 및 현실 세계와 가까운 FER 데이터셋 (통제되지 않은 상태에서 수집, 주로 일부가 가려진 얼굴 이미지)에 대해 평가함 
- 일반적인 FER 데이터셋 (CK+, MMI, Oulu-CASIA) (In-the-Lab Datasets) 
- 현실 세계와 가까운 FER 데이터셋 (RAF-DB, AffectNet, SFEW) (In-the-Wild) 
- 가려진 이미지에서의 성능 확인을 위해 임의로 이미지의 절반 크기에 해당하는 물체로 얼굴 일부를 가림 (약 4000개 샘플) 

## 의견
- 각 이미지에 대해 Facial Landmarks에 따라 24개의 ROI 패치를 계산하고 추출해야 하는 수고가 따름 
- 가려진 정도 (or 중요도) 가중치를 학습하기 위한 레이블이 모든 이미지 패치에 대해 필요함 
(하나의 이미지에 24개의 패치 존재) 
