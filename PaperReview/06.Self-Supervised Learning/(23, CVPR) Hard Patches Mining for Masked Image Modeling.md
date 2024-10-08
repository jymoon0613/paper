# Hard Patches Mining for Masked Image Modeling (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2304.05919)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2023hard.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Masked image modeling(MIM)은 효과적인 self-supervised learning 방법 중 하나이며 최근 많이 연구되고 있음
- MIM에서 모델은 보통 masked patches를 reconstruction하도록 훈련됨
- 이미지의 redundancy한 특징을 완화하면서 더 어려운 pretext task를 설정하기 위해 마스킹 전략을 잘 설정하는 것이 중요하며 보통 random masking과 같이 사전에 정의된 방식으로 마스킹이 수행됨
- 하지만, 단순히 어려운 task를 사전에 정의해서 모델이 학습하도록 하는 것이 아니라 모델이 자체적으로 task를 더 어렵게 만드는 방법을 학습하게 하는 것이 중요함
> - 즉, 모델은 스스로 어려운 task를 만들고, 그 task를 해결하면서 학습함
> - 이를 통해 모델은 이미지 내용에 대한 보다 포괄적인 이해를 할 수 있음
- 따라서, 본 논문에서는 주어진 이미지에 대해 모델이 적절한 마스크를 계산하고, 모델은 해당 마스크로 마스킹된 이미지 패치를 다시 reconstruction하도록 훈련하는 새로운 MIM 학습 패러다임을 제안함 (Hard Patches Mining(HPM))

## 접근법
- 사전 훈련된 모델로 시각화를 진행한 결과 classification에 중요한 object 정보들의 reconstruction loss가 컸음
- 이를 바탕으로 모델이 각 패치에 대한 예상 reconstruction loss를 예측하고, 높은 예측 reconstruction loss를 지니는 패치들을 마스킹하는 방법을 사용함
- HPM은 student(student encoder, reconstructor, reconstruction loss predictor)와 teacher(teacher encoder, reconstructor, reconstruction loss predictor)로 구성됨
> - Teacher는 student로부터 momentum update됨
- 먼저, 입력 이미지는 patch embedding 후 teacher로 입력되고, teacher는 패치별 predicted reconstruction loss를 계산함
- Teacher의 predicted reconstruction loss를 바탕으로 마스킹을 위한 binary mask를 구함
- Student는 생성된 binary mask로부터 마스킹된 이미지 패치를 reconstruction하도록 훈련되며(reconstruction loss), 패치별 reconstruction loss 예측에 대한 predicting loss도 계산하여 같이 사용함
- 패치별 reconstruction loss 예측에 대한 predicting loss를 정의하는 가장 간단한 방법은 실제 decoder의 패치별 reconstruction loss와 auxiliary decoder가 예측한 패치별 reconstruction loss의 MSE loss를 계산하는 방법임
> - 하지만 이 경우, decoder의 패치별 reconstruction loss는 학습이 진행됨에 따라 지속적으로 감소하는 것에 반해, predicting loss가 측정해야 하는 것은 패치별 상대적인 reconstruction 난이도임
- 따라서, 본 논문에서는 binary cross-entropy 기반의 relative loss를 predicting loss로 사용함
- 즉, 주어진 decoder의 패치별 실제 reconstruction loss의 argument sort 결과를 예측하도록 함
> - 하지만 argument sort 연산은 미분불가능함
> - 따라서, 주어진 문제를 binary cross-entropy 기반의 dense relation comparision 방식으로 치환하여 relative loss를 구현함
- Teacher가 패치별 predicted reconstruction loss를 계산하면 argsort 연산을 사용하여 masking 될 패치 위치를 결정함
- 하지만, 훈련 초기에는 간단한 texture 정보에 reconstruction loss가 집중되게 되고, 이는 설계의 근거가 되었던 reconstruction loss와 discriminative가 상관관계가 있다는 가정에서 벗어남
- 따라서, easy-to-hard mask generation 방법을 제안하여 hard masked patches를 점진적으로 늘리도록 가이드함
> - 학습 초기에는 predicted reconstruction loss에 기반하여 마스킹되는 패치의 수를 적게 하고, 나머지는 랜덤하게 마스킹함
> - 이후 학습이 진행됨에 따라 predicted reconstruction loss에 기반하여 마스킹되는 패치의 수를 증가시킴

## 실험결과
- Classification과 detection, segmentation tasks에서 모델을 검증함
- Reconstruction target, masking strategy, prediction loss의 formulations에 대한 ablation을 수행

## 의견
- /