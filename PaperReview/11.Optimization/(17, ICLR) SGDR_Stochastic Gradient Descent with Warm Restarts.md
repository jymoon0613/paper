# SGDR: Stochastic Gradient Descent with Warm Restarts (ICLR, 2017)

[논문링크](https://arxiv.org/abs/1608.03983)

<p align="center">
    <img width="600" alt='fig1' src="../img/loshchilov2016sgdr.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 SGD의 learning rate를 주기적으로 특정 값으로 restart하고 감소시키는 방법을 제안함 (SGDR)

## 접근법
- 특정 epoch에 도달하면 SGD의 learning rate는 restart됨
> - Restart-Decrease를 반복함
> - Decrease는 cosine annealing schedule로 수행됨
- 최대/최소 learning rate를 결정해야 함
> - Learning rate가 설정한 최소값에 도달하면 사이클 종료
> - Learning rate는 다시 최대값으로 설정되고 사이클 시작

## 실험결과
- SGDR을 사용했을 때 네트워크의 error가 더욱 감소됨
- SGDR은 learning rate를 최소/최대값 사이에서 급격히 감소시켰다 증가시키므로 local minimum을 빠르게 벗어날 수 있다는 장점이 있음
> - 모델의 일반화 성능을 극대화

## 의견
- /