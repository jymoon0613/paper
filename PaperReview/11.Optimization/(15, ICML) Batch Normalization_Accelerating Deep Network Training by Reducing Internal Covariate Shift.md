# Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift (ICML, 2015)

[논문 링크](http://proceedings.mlr.press/v37/ioffe15.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/ioffe2015batch.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Stochastic Gradient Descent(SGD)와 Adagrad 같은 SGD의 변형 방법들은 여러 deep learning 모델의 최적화에 좋은 성능을 보여줌
- SGD를 사용할 때는 초기화 및 학습률 설정이 매우 중요한데, 이는 한 레이어의 입력이 모든 이전 레이어의 파라미터에 의해 영향을 받기 때문이며, 깊은 네트워크의 경우 파라미터의 아주 작은 변화라 할지라도 레이어를 거쳐갈수록 점점 증폭됨
- 이렇게 레이어를 통과할 때마다 이전 레이어의 파라미터 변화로 인하여 현재 레이어의 입력의 분포가 바뀌는 현상을 internal covariate shift라고 부름 (covariate shift: 네트워크 입력의 분포가 바뀌는 현상)
- 레이어의 입력 분포가 일정하게 유지되는 것이 중요한데, 이는 레이어의 학습 과정에서 입력 분포 변화에 의한 불필요한 파라미터 조정 과정을 피할 수 있기 때문임
- 또한 레이어의 입력 분포를 일정하게 유지시키는 것은 학습하는 과정 자체를 안정화시켜 더욱 빠른 수렴을 가능하게 함
- 따라서, 본 논문에서는 internal covariate shift 현상을 제거하여 전체적인 네트워크 수렴을 가속화할 수 있는 Batch Normalization 기법을 제안함
- Batch Normalization은 각 레이어 입력 분포의 평균과 분산을 일정하게 유지시키는 역할을 함

## 접근법
- 레이어의 입력 분포를 일정하게 유지시키는 가장 간단한은 각 레이어의 입력을 평균 0, 분산 1로 whitening 시켜주는 것
- 하지만, whitening의 경우 covariance matrix의 계산과 inverse의 계산이 필요하기 때문에 계산량이 많을 뿐더러, 일부 파라미터들의 영향을 무시함
- 또한 whitening은 최적화(Backpropagation)과정과 무관하게 진행됨
- 따라서 단순하게 whitening만을 시킨다면 특정 파라미터가 계속 커지는 상태로 whitening이 진행됨
- 배치 정규화는 평균과 분산을 조정하는 과정이 별도의 과정으로 떼어진 것이 아니라, 신경망 안에 포함되어 학습 시 평균과 분산을 조정하는 과정 역시 같이 조절됨
- 즉, 각 레이어마다 정규화 하는 레이어를 두어, 변형된 분포가 나오지 않도록 조절
- 배치 정규화는 mini-batch의 평균과 분산을 이용해서 입력을 정규화 한 뒤에, scale 및 shift를 학습 가능한 변수인 감마, 베타를 통해 조절
- 학습 시에는 배치 정규화의 미니 배치의 평균과 분산을 이용 할 수 있지만, 테스트(추론) 할 때에는 이를 이용할 수 없음
- Inference 시에 입력되는 값을 통해서 정규화를 하게 되면 모델이 학습을 통해서 입력 데이터의 분포를 추정하는 의미 자체가 없어지며 배치 정규화가 온전하게 이루어지지 않음
- 따라서, inference 시에는 결과를 deterministic 하게 하기 위하여 고정된 평균과, 분산을 이용하여 정규화를 수행
- 이때 고정된 평균과 분산은 학습 과정에서 이동 평균(moving average) 또는 지수 평균(exponential average)을 통하여 계산한 값
- Batch Normalization은 non-linear activation 전에 수행됨

## 실험결과
- MNIST 데이터셋에서의 분류 성능으로 평가한 결과 Batch Normalization은 학습을 더욱 빠르게 해주며 정확도도 더 높았음
- Batch Normalization은 네트워크가 초기값 설정에 덜 민감하도록 해주며, 더 큰 학습률을 설정하더라도 안정적으로 수렴할 수 있도록 해줌
- 또한, Batch Normalization은 모델을 regularize하는 역할을 하여 Dropout 등의 기법을 사용하지 않아도 됨 
- 입력의 범위가 고정되기 때문에 saturating 한 함수를 활성화 함수(ex. sigmoid)로 써도 saturation 문제가 일어나지 않음

## 의견
- /