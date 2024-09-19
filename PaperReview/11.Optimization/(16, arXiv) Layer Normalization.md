# Layer Normalization (arXiv, 2016)

[논문링크](https://arxiv.org/abs/1607.06450)

<p align="center">
    <img width="800" alt='fig1' src="../img/lei2016layer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Batch normalization(BN)은 deep neural networks의 training time을 효과적으로 단축시켜주었음
> - 모델의 빠른 수렴을 촉진시키며 regularizer 역할도 수행
- BN을 구현하기 위해서는 summed input statistics의 runnning averages가 필요하고, 일반적인 neural networks에서는 각 hidden layer에 statistics를 축적해나감으로써 계산할 수 있음
- 하지만 시퀀스 데이터를 다루는 RNN의 경우, summed input statistics는 입력 시퀀스의 길이에 따라 상이해질 수 있으며 이는 time-steps에 따라 서로 다른 statistics가 요구됨을 의미함
> - 또한, test input의 시퀀스가 training input의 시퀀스보다 길 때 문제가 될 수 있음
> - 즉, BN을 RNN에 적용하는 방법이 명확하지 않음
- 또한, BN의 효과는 batch size에 따라 달라지고 배치 사이즈가 매우 작아질 수 있는 online learning tasks나 distributed learning tasks의 경우 적용이 어려움
- 따라서, 본 논문에서는 BN의 단점을 보완한 layer normalization(LN)이라는 새로운 normalization 기법을 제안함

## 접근법
- BN은 batch 내의 모든 samples에 대해 각 feature의 평균과 분산을 구해서, 모든 samples의 각 features를 정규화함
- LN은 batch 내의 모든 samples 각각의 평균과 분산을 구해서, 모든 samples 각각을 정규화함 (그림 참고)
> - LN의 경우 batch 내의 samples 간의 의존성은 고려되지 않으므로, batch size의 제약에서 자유로움

## 실험결과
- LN을 사용하면 LN을 사용하지 않았을 때보다 수렴이 빨랐음
- 여러 tasks에서 BN보다 수렴이 빨랐고 성능 증가도 더 컸음

## 의견
- /