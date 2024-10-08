# Learning Attentive Pairwise Interaction for Fine-Grained Classification (AAAI, 2020)

[논문 링크](https://ojs.aaai.org/index.php/AAAI/article/view/7016)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhuang2020learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Visual Classification (FGVC)는 어려움
- 대다수의 선행 연구는 하나의 이미지에 대한 local parts를 찾는 것에 집중함
- 하지만 인간은 종종 서로 다른 이미지 쌍을 비교하여 subtle difference를 찾고 object를 인식함
- 이미지 쌍의 비교를 통해 object에 대한 깊은 이해가 가능함
- 이러한 인간의 인지 능력에 기반하여 fine-grained images 쌍의 대조적인 특징을 발견하고 이미지 쌍의 상호작용을 통해 object를 주의깊게 식별하는 Attentive Pairwise Interaction Network (API-Net)을 제안함

## 접근법
- 먼저 쌍을 이루는 두 이미지는 CNN 백본을 거쳐 각각의 피처 벡터로 projection됨
- 두 피처 벡터는 MLP를 거쳐 하나의 mutual vector로 변환됨
- Mutual vector는 두 이미지의 대조적 특징들을 담고 있음
- Mutual vector와 두 이미지 벡터 각각의 point-wise multiplication 및 sigmoid를 통해 gate vecotr를 만들어냄
- Gate vector는 각 이미지의 중요한 부분과 쌍 이미지와 차별점이 있는 부분을 모델링하는 역할을 함
- 자기 자신의 피처 벡터와 gate vector 간의 point-wise multiplication으로 'self' 벡터 생성
- 자신의 피처 벡터와 쌍 이미지의 gate vector 간의 point-wise multiplication으로 'other' 벡터 생성
- 생성된 4개의 피처 벡터로 분류를 수행하고 loss를 계산
- 또한, score ranking regularization을 위해 hinge loss를 사용함
- Object 분류에는 self 벡터보다 other 벡터가 더 중요하므로 모델이 self에 더 집중하도록 hinge loss로 규제함
- Inference 시에는 훈련된 CNN 백본만 사용함 (일종의 pre-train)

## 실험결과
- CUB-200-2011, Aircraft, Stanford Cars, Stanford Dogs, NABirds 데이터셋에서 검증함
- ResNet=101을 백본으로 사용
- Method 구조에 대한 ablation 수행
- 여러 접근법들의 기존 성능을 뛰어넘음

## 의견
- 일종의 pre-train 방식으로 method 사용
- 비슷한 접근법이므로 어떤 실험 진행하고 결과는 어떻게 보여주는지 참고하면 좋을듯
- 각각의 데이터셋에 대해 여러 선행 연구를 카테고리화 하고 각 카테고리 별로 어떤 점에서 제안하는 방법이 더 나은지를 설명하면서 평가
- Ablation과 visualization 적극 활용
