# GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism (NIPS, 2019)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2019/hash/093f65e080a295f8076b1c5722a46aa2-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/huang2019gpipe.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Larger models로 scaling하는 것은 정확도 향상에 큰 기여를 보여주었지만 GPU나 TPU에서의 memory limitations나 communication bandwidth 같은 hardware constraints라는 어려움이 있음
- 본 논문에서는 large neural networks를 효율적으로 훈련시키는 flexible한 라이브러리인 GPipe를 제안함
> - Model partitioning과 re-materialization 기법을 사용

## 접근법
- 하나의 네트워크를 구성하는 $L$개의 layers를 $K$개의 partitions 혹은 cells로 분할함
- 각 cell은 하나의 accelerator(e.g., GPU, TPU)에 배정됨
- $N$ 사이즈의 mini-batch가 동일한 $M$ 사이즈의 micro-batches로 나누어져 $K$개의 accelerator를 통과함
> - 여러 파티션이 최대한 병렬적으로 작동할 수 있도록 함
- 모든 micro-batches에 대한 gradients를 축적하여 모델 파라미터를 업데이트
- Activation memory를 줄이기 위해 re-materialization 기법을 사용
> - Forward propagation 시 각 accelerator는 partition 경계에서의 output activation만 저장
> - Backward propagation 시 각 accelerator가 forward function을 다시 계산
> - Peak activation memory requirement를 줄일 수 있음

## 실험결과
- GPipe를 적용하면 large model의 처리 속도가 크게 개선

## 의견
- /