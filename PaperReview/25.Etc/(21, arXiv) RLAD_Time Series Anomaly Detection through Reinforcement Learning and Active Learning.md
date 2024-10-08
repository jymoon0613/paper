# RLAD: Time Series Anomaly Detection through Reinforcement Learning and Active Learning (arXiv, 2021)

[논문링크](https://arxiv.org/abs/2104.00543)

<p align="center">
    <img width="400" alt='fig1' src="../img/wu2021rlad.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Anomaly detection은 anomaly samples가 적고 anomalies가 non-stationary하다는 문제가 있음
> - Anomalies는 빈번하게 나타나지 않으므로 훈련을 위한 sample이 적음
> - Anomalies는 시간이 지남에 따라 평균, 분산과 같은 통계적 특성이 변함
- 본 논문에서는 reinforcement learning과 active learning을 결합한 RLAD를 제안함

## 접근법
- 데이터를 window로 분할
> - 하나의 window가 하나의 state
- iForest 모델을 사용하여 데이터로부터 anomaly score 계산하여 분류
> - DQN의 replay memory에 저장
- DQN 사용하여 reinforcement learning 수행
> - Evluation/target network는 LSTM을 사용
- Lablel propagation으로 semi-supervsised learning 수행
> - 학습 anomaly samples 부족 문제 해결
- Active learing으로 non-stationary 문제에 대처

## 실험결과
- 다른 anomaly detection methods보다 높은 성능 (semi-supervised, unsupervised)

## 의견
- /