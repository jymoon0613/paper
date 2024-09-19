# A Discriminative Feature Learning Approach for Deep Face Recognition (ECCV, 2016)

[논문 링크](https://link.springer.com/chapter/10.1007/978-3-319-46478-7_31)

<p align="center">
    <img width="400" alt='fig1' src="../img/wen2016discriminative.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN은 일종의 Feature Extractor로서 주어진 입력 이미지에 대응하는 클래스를 예측하기 위해 어떤 피처들에 집중해야 하는지를 학습함 
- 좋은 피처 학습이란 모델이 클래스 별 구별 가능한 피처 (Separable)를 추출하는 것을 넘어, 클래스 별 차별점이 명확한 피처(Discriminative)를 추출할 수 있어야 함 
- 본 논문에서는 Center Loss라는 손실 함수를 제안하여, 기존의 Softmax Loss와 함께 사용함으로써 Softmax는 클래스 간 피처를 Separable하게 하고 (Push), Center Loss는 클래스 간 피처를 Discriminative하게 함으로써 (Pull) 모델의 깊은 피처 학습을 돕는 방안을 제시한다. 
- Softmax Loss = Push = High Inter-class difference 
- Center Loss = Pull = Low Intra-class variation 

## 접근법
- Clustering의 Inertia와 같이 각 입력 데이터에 대해 해당하는 클래스의 중심과의 거리가 작아지도록 학습하는 방법이 있음 (매 학습마다 클래스 중심 위치 업데이트, Intra-class Variation 감소) 
- 하지만, 이를 위해 모든 학습 데이터를 한 번에 사용해야 한다는 한계가 존재 (Batch Size = Training Set) 
- 이를 해결하기 위해, 클래스 중심을 모든 데이터셋이 아닌 Mini-batch 단위로 업데이트 
- 클래스 중심 좌표는 학습 가능한 형태로 정의하여 매 반복마다 업데이트 
- 기존의 Softmax Loss와 Center Loss를 조합하여 Loss Function을 구성 

## 실험결과
- 어려운 Classification Task인 Facial Recognition Datasets에서 성능을 검증함 
- 일반 Softmax Loss, Softmax Loss + Contrastive Loss, Softmax Loss + Center Loss로 비교평가 진행 

## 의견
- 성능을 더욱 개선시킬 방법은? 
- 우리의 태스크에 맞게 혹은 Attention Network에 맞게 변형시킬 방법은? 
