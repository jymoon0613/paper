# SqueezeNet: AlexNet-level Accuracy with 50x Fewer Parameters and <0.5MB Model Size (arXiv, 2017)

[논문링크](https://arxiv.org/abs/1602.07360)

<p align="center">
    <img width="600" alt='fig1' src="../img/iandola2016squeezenet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 작은 CNN 모델(파라미터 수가 적은)은 (1) 분산 처리를 위한 communication이 적어 더 빨리 학습 가능하고, (2) 빠른 실시간 고객 정보 전송 및 고객을 위한 버전 업데이트에서 유리하며, (3) embeded 시스템에 적용이 용이하다는 장점이 있음
- 따라서, 본 논문에서는 파라미터 수를 낮추고 정확도는 유지시키는 방법에 집중함 (SqueezeNet)

## 접근법
- 정확도를 유지하면서 사이즈를 줄이기 위해 3가지 디자인 전략을 사용함
> - 3x3 convolution filter의 대부분을 1x1 filter로 변경 (9배 적은 파라미터 수)
> - 3x3 convolution layer로 입력되는 채널의 수를 줄임: 'squeeze layer'를 이용
> - Downsampling을 네트워크의 후반부에 집중시킴: 중간 레이어에서는 large activation map을 유지하며, 이러한 large activation map은 성능에 중요함
- SqueezeNet은 위의 세 가지 전략이 반영된 block인 Fire module로 구성됨
- Fire module은 1x1 convolution으로만 구성된 squeeze layer와 1x1 및 3x3 convolution이 혼합되어 있는 expand layer로 구성됨
- 1x1 convolution으로만 구성된 squeeze layer는 위의 첫번째 전략을 반영
- Fire module은 squeeze layer의 1x1 convolution filter의 수($s_{1\times1}$), expand layer의 1x1 convolution filter의 수($e_{1\times1}$) 및 3x3 convolution filter의 수($e_{3\times3}$) 총 세 가지 파라미터를 지님
- 이때 $s_{1\times1}$을 $e_{1\times1}+e_{3\times3}$보다 항상 작도록 설정하여 두번째 전략을 반영함
- SqueezeNet은 pooling 연산을 가능한 후반부에 배치하여 세번째 전략을 반영함

## 실험결과
- AlexNet과 비교했을 때 SqueezeNet은 파라미터 수를 크게 감소시켰으며 비슷한 혹은 더 나은 정확도를 기록함
- Fire module에 bypass(or skip-connection)을 적용했을 때 성능이 더 증가함

## 의견
- /