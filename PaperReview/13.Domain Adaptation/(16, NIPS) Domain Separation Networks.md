# Domain Separation Networks (NIPS, 2016)

[논문 링크](https://proceedings.neurips.cc/paper/2016/hash/45fbc6d3e05ebd93369ce542e8f2322d-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/bousmalis2016domain.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근 Supervised Learning의 좋은 성능은 매우 큰 데이터셋의 영향에 기반함 
- 하지만, 이러한 큰 데이터셋을 구축하는 데에는 많은 시간과 비용이 소요되므로, 이미지 생성과 같은 대체 방법이 제안되기도 함 
- 하지만, 가상 데이터를 통해 학습된 모델은 실제 도메인 데이터에 대해 잘 일반화되지 못한다는 단점이 있음 
- 따라서, 본 연구에서는 충분한 Label이 있는 Source Data로부터의 학습된 지식을 Ground Truth Label이 없는 Target Data로 잘 전이시키기 위해 Domain-invariant Representation을 학습하는 문제를 다룸 
- 특히 본 연구에서는 Source Domain과 Target Domain 모두에서 주의를 두어야 할 객체가 Foreground에 위치하는 이미지 분류와 Pose Estimation Task에서의 Domain Adaptation 문제에 집중함 
- Source Domain과 Target Domain은 노이즈, Resolution, Illumination, Color와 같은 Low-level 측면에서 차이가 있으며, Class의 수, 객체의 종류와 같은 High-level 측면에서는 상당한 유사성을 보인다고 가정함 
- 본 연구에서는 Domain-invariant Representation을 학습하는 아키텍처인 Domain Separation Networks (DSN)을 제안함 
- 이전의 연구에서는 Source Domain의 표현에서 Target Domain의 표현으로의 매핑을 찾거나, 두 Domain 사이에 공유되는 표현을 찾으려는 시도가 있었지만 이는 공유 분포 (Shared Distribution)와 관련된 노이즈에 취약하다는 문제가 있었음 
- 반면에 DSN은 각 Domain의 고유한 특징을 찾으며, 이러한 Domain 고유의 속성과 수직이 되는 공유 공간을 찾음으로써 공유 특징과 고유한 특징을 분리할 수 있고 더 의미 있는 표현을 생성할 수 있음 
- DSN은 이미지 분류와 Pose Estimation에서 다른 SOTA 알고리즘을 능가했을 뿐만 아니라, 공유 특징과 고유 특징의 시각화를 통한 해석의 용이성 또한 지님 

## 접근법
### (1) Overview 
- 목적은 Label 데이터가 포함된 Source Domain으로부터 Label 데이터가 없는 Target Domain에 일반화될 수 있는 표현을 학습하는 것 
- 다른 이전의 방법과 마찬가지로, 모델은 Source Domain의 Representation이 Target Domain의 Representation과 유사해지도록 훈련됨 
- 이는 일반화할 수 있지만, 공유된 Representation과 높은 상관 관계가 있는 노이즈가 포함되어 있을 수 있음 
- DSN은 Domain 간의 공유되는 표현과 Domain 각각의 고유한 표현 둘 다 명시적으로 모델링함 
- 이렇게 공유 표현과 개별 표현을 분리하게 되면, 공유 표현에 대해 훈련된 분류기는 입력이 각 Domain에 고유한 표현의 측면으로 오염되지 않기 때문에 Domain 전반에 걸쳐 더 잘 일반화할 수 있음

### (2) Learning 
- Source Domain 데이터에 대해 Shared Encoder를 거치고 Classification Layer를 거쳐 분류를 수행한 뒤 Classification Loss 계산 (Target Domain은 Label이 없으니 Source에 대해서만 계산) 
- Private Encoder가 Trivial Solution이 아닌 유용한 정보를 담고 있도록 하고 일반화 가능성을 증진시키기 위해 Reconstruction Loss를 사용 
- Reconstruction Loss는 모델이 입력의 절대 색상이나 강도에 대한 모델링 능력을 허비하지 않고 모델링 되는 객체의 전체 모양을 Reconstruction하는 방법을 학습하도록 Scale-invariant MSE 사용 
- Private Representation과 Shared Representation가 최대한 서로 다른 표현을 나타내도록 Difference Loss를 사용 (두 표현 사이의 직교성을 제약으로 사용함) (Why Frobenius Norm? 절대값 제곱의 합을 이용하여 내적 값이 0에 가까워지도록 함) 
- 마지막으로 Source Domain과 Target Domain의 표현을 최대한 유사해지도록 하기 위해 Similarity Loss를 사용함 (Shared Representation 이므로) 

### (3) Similarity Loss 
- Domain Adversarial Similarity Loss는 분류기가 주어진 표현이 Source의 표현인지 Target의 표현인지 구분하지 못하도록 훈련 (Binary-Cross Entropy를 사용하여 분류를 잘하도록 훈련하지만 Gradient Reversal Layer를 통해 반대로 훈련시키므로 구분을 못하도록 훈련됨) 
- Maximum Mean Discrepancy는 두 분포 사이의 차이를 측정, Kernel Function으로는 여러 RBF Kernel의 Combination을 사용함 

## 실험결과
- (a) MNIST to MNIST-M, (b) Synthetic Digits to SVHN, (c) SVHN to MNIST, (d) Synthetic Traffic Signs to GTSRB, (e) Synthetic LINEMOD to Real LINEMOD 총 5개의 시나리오에서 검증함 
- CORAL, MMD, DANN과 비교를 진행했으며, 이외에도 Source Domain과 Target Domain에서 Domain Adaptation 없이 각각 일반적으로 훈련되었을 때의 Target Domain에서의 성능을 제시 
- 제안하는 DSN, 특히, Similarity Loss로 DANN을 사용했을 때 성능이 가장 좋았음 
- 시각화를 진행했을 때, 모델은 Source Domain과 매우 유사한 공유 표현을 학습함 

## 의견
- 단순히 Source Domain과 Target Domain 간의 공유되는 공간에만 집중하는 것이 아니라, 모델이 두 Domain에 고유한 표현 공간을 먼저 찾고, 이러한 고유 공간에 직교하는 공유 공간을 찾도록 함으로써, 기존의 공유 공간 노이즈 문제를 해결하면서 모델의 성능을 개선함 
- Domain의 Private한 속성에 의해 오염되지 않는 분리된 공유 공간을 탐색 (유용함 증진) 

