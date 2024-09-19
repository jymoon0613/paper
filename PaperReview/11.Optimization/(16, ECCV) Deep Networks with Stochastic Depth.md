# Deep Networks with Stochastic Depth (ECCV, 2016)

[논문링크](https://ui.adsabs.harvard.edu/abs/2016arXiv160309382H/abstract)

<p align="center">
    <img width="600" alt='fig1' src="../img/huang2016deep.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 네트워크의 layers 증가는 representational power의 증가로 이어지지만, vanishing gradients, diminishing feature reuse, long training time이라는 문제를 야기함
- 본 논문에서는 training 시에는 short network처럼 학습시키고, test시에는 deep network의 장점을 누릴 수 있는 deep networks with stochastic depth(DNSD)라는 학습 알고리즘을 제안함
> - 학습 시 네트워크의 일부 layers를 랜덤하게 제거함
> - Dropout이 같은 depth의 hidden nodes를 일부 제거하는 것이라면, DNSD는 서로 다른 depth의 layers를 일부 제거하는 것
> - Dropout은 네트워크를 'thin'하게 한다면, DNSD는 네트워크를 'short'하게 함
> - 또한, Dropout은 Batch Normalization과 함께 사용될 때 그 효과를 잃지만, DNSD는 Batch Normalization과 함께 사용할 수 있음

## 접근법
- ResNet과 같은 맥락으로 Skip-connection을 사용하여 DNSD를 구현할 수 있음
- 단, DNSD는 mini-batch마다 랜덤하게 connection pattern을 변경함
> - Mini-batch마다 일부 layers를 샘플링하고, 샘플링된 layers는 identity mapping만을 수행하도록 함
> - ResNet을 기준으로, ResNet block 단위로 샘플링하여 drop함
- Survival probabilities를 정의하고, linear decay rule로 값을 변경하여 네트워크의 후반부로 갈수록 더 많은 blocks가 drop되게 함

## 실험결과
- DNSD를 사용했을 때 baseline보다 오히려 더 좋은 성능을 기록했음
> - DNSD가 ResNet blocks 간의 implicit한 model ensemble 효과를 주기 때문임
- Mini-batch마다 50%의 blocks를 drop하는 경우 학습 시간이 25% 빨라짐

## 의견
- /