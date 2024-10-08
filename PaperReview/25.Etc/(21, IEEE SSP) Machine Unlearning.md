# Machine Unlearning (IEEE SSP, 2021)

[논문링크](https://arxiv.org/abs/1912.03817)

<p align="center">
    <img width="600" alt='fig1' src="../img/bourtoule2021machine.png?raw=true"></br>
    <em><font size=2>Machine Unlearning.</font></em>
</p>

## 연구목적
- Machine unlearning이란 모델이 학습한 정보나 패턴을 지우거나 대체하기 위한 과정을 의미함
  - 의도치 않게 학습된 데이터에 포함된 특정 개인 정보를 삭제하는 개인정보 보호 관련 활용 가능
  - 시간의 흐름이나 상황의 변화로 인해 더 이상 유효하지 않은 데이터를 잊고 새로운 정보를 받아들일 수 있도록 재학습시키기 위해 활용 가능
- 본 논문에서는 machine unlearning을 위한 shared, isolated, sliced, aggregated(SISA) training 기법을 제안함

## 접근법
- SISA는 distributed training 방법을 사용하여 원본 모델을 다수의 constituent model로 복제하고, 복제된 constituent는 각각 shard으로 학습됨
  - Shard는 데이터셋의 subset으로서 각 shard는 서로 disjoint함
  - Shard는 다시 slice라는 단위로 쪼개지며, 각 constituent는 shard를 구성하는 slice 단위로 점진적인 학습이 수행됨
  - 각 constituent에서 계산된 gradients는 다른 constituents와 공유되지 않음
  - 즉, 하나의 shard가 영향을 주는 대상은 해당 shard가 배정된 constituent가 유일함을 보장함
- Inference 시에는 각 constituent가 입력에 대한 예측을 수행하며, 모든 constituents의 responses를 aggregate하여 최종 결과를 도출함
- Unlearning이 요청되면 해당 data point를 포함하는 shard로 훈련된 constituent만 영향을 받음
  - 지정된 constituent의 가중치를 타겟 data point가 포함된 slice 학습 전으로 초기화한 후 해당 slice를 제외하여 재학습 진행

## 실험결과
- Unlearning의 성능적 목표는 gold standard보다 빠르게 재학습을 하면서 최대한 비슷한 성능 수준을 유지하는 것임
  - Gold standard: 주어진 모델을 타겟 data point가 없는 데이터셋으로 다시 처음부터 학습한 모델
- SISA는 다른 unlearning 방법보다 빠르면서 정확도 유지에도 탁월함

## 의견
- /