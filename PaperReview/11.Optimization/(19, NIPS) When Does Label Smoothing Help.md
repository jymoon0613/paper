# When Does Label Smoothing Help? (NIPS, 2019)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2019/hash/f1748d6b0fd9d439f71450117eba2725-Abstract.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/muller2019does.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Label smoothing은 네트워크가 hard targets에 대해 학습하는 것이 아니라 targets의 weighted mixture를 학습하도록 함으로써 성능을 증진시키는 방법임
- 본 논문에서는 label smoothing이 네트워크 학습에 어떤 영향을 주는지 탐구하고자 함

## 접근법 및 실험결과
- Label smoothing은 네트워크의 마지막 activation layer의 출력이 correct class에 가깝도록 하고, incorrect한 classes와는 일정한 크기로 멀어지도록 하는 효과가 있음
> - Label smoothing을 사용할 때와 안할 때의 activation을 비교함
> - Label smoothing을 사용하지 않을 때는 activation이 target class를 중심으로 분포하긴 하지만 많이 퍼져 있음
> - Label smoothing을 사용할 때는 훨신 tight한 class cluster가 생성되고, 이는 label smoothing이 incorrect classes에 대해 일정한 크기로 멀어지도록 강제하기 때문임
- Label smoothing은 모델이 correct class에 over-confidence하는 경향을 막아줌
> - 또한, 오히려 correct class에 더 잘 집중하도록 네트워크를 calibrate하는 효과가 있으며, 일반화도 잘됨
- 하지만, label smoothing을 사용하여 학습한 네트워크는 더 좋은 성능을 보여주었지만, 학습된 네트워크를 knowledge distillation의 teacher 모델로 사용하면 hard target을 사용한 teacher를 사용할 때보다 student 모델의 성능이 낮았음 
> - Label smoothing과 knowledge distillation은 모두 soft targets을 사용하여 모델을 fitting함 (knowledge distillation의 경우 softmax temperature T를 사용)
> - 따라서, teacher network를 훈련시킬 필요가 없는 label smoothing을 사용하는 것이 더 효과적이라고 할 수 있고, knowledge disillation은 label smoothing과 비교했을 때 추가적인 이점이 있는 경우에만 사용하는 것이 효과적

## 의견
- /