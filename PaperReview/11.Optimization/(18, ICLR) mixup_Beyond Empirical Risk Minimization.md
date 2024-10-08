# mixup: Beyond Empirical Risk Minimization (ICLR, 2018)

[논문링크](https://arxiv.org/abs/1710.09412)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2017mixup.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 거의 모든 nueral networks는 주어진 훈련 데이터셋에 대한 empirical rist를 minimization하는 방식으로 훈련됨 (ERM)
- 하지만 ERM 기반의 학습 시 아무리 강력한 regularization 기법을 사용하더라도 네트워크가 훈련 데이터를 학습하기보다는 단순히 기억하여 훈련 데이터에 overfitting되는 현상이 발생한다는 문제가 있음
- 따라서, ERM을 기반으로 학습한 네트워크는 OOD(Out Of Distribution) 데이터에 취약함
- 이는 ERM 기반의 학습이 네트워크가 오직 훈련 데이터셋과 약간 다른 분포를 가진 시험 데이터셋에서만 일반화를 달성할 수 있게 한다는 것을 의미함
- 이에 새로운 방법인 Vicinal Risk Minimization(VRM) principle이 등장함: 훈련 데이터셋만 학습하는 것이 아니라 훈련 데이터셋의 근방(vicinal) 분포도 함께 활용 = Data Augmentation
- 하지만, 기존의 data augmentation은 어떤 데이터셋이냐에 영향을 많이 받기 때문에 expert knowledge가 요구되고, 같은 클래스의 vicinity에만 국한되어 서로 다른 클래스 간의 연관된 vicinity는 모델링하지 못한다는 단점이 있음
- 따라서, 본 논문에서는 간단하고 data-agnostic한 augmentation 기법인 mixup을 제안함

## 접근법
- 두 샘플과 레이블을 샘플링하고, 각각을 일정 비율로 섞은(mixup) 새로운 샘플과 레이블을 사용하여 훈련함
- 세 개 이상의 샘플을 섞는 것은 추가적인 성능 이점이 없었고 오히려 연산량을 증가시킴
- 실제 구현에서는 하나의 data loader를 사용하며, 하나의 미니 배치를 샘플링한 후 동일한 미니 배치에 random shuffling을 적용하여 mixup을 진행함 (성능 동일, 연산량 감소)
- 같은 레이블을 지니는 샘플 사이에서만 mixup을 진행하는 것은 성능 향상을 가져오지 않음
- Mixup 은 모델이 training data 간에 선형적인 양상(linear behavior) 을 가지게끔 해줌
> - Linear behavior는 training data 이외의 데이터에 대한 예측에서 예상치 못한 결과물(undesirable oscillations)을 내뱉는 것을 줄여줌
> - Occam's razor에 따라 linearlity는 좋은 inductive bias임
> - 불확실성에 대한 더 부드러운 estimate을 제공
> - 학습을 안정화해줌

## 실험결과
- Mixup은 overfitting을 완화
- Adversarial examples에도 더욱 강건함
- Mixup을 사용한 GAN은 hyper-parameter와 아키텍처 선택에 강건하며 학습이 더욱 안정화됨

## 의견
- /