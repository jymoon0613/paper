# An Empirical Study of Training Self-Supervised Vision Transformers (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Chen_An_Empirical_Study_of_Training_Self-Supervised_Vision_Transformers_ICCV_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/chen2021empirical.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN을 훈련시키는 방법에 대한 논의는 충분히 진행되었지만 비교적 최근에 제안된 ViT의 훈련 방법에 대한 연구는 아직 부족함
- 따라서, 본 논문에서는 최근의 siamese networks 구조를 사용하여 ViT를 pre-training하는 방법에 대해 논의함
- 궁극적으로, MoCov1/v2의 개선된 버전인 MoCov3를 제안함

## 접근법
- MoCov3는 기존의 MoCo에서 queue를 사용하지 않으며 SimCLR와 동일하게 배치 사이즈를 키우고 하나의 배치 내에서 negative samples를 사용함
- Encoder는 SimSiam에서 제안한 backbone + projector + predictor를 사용하며, momentum encoder는 backbone + projector를 사용함
- Momentum update를 사용하는 방식은 동일함
- 추가적인 prediction head 및 큰 배치 사이즈는 MoCov3가 MoCov1/v2의 ResNet-50에서의 linear evaluation 성능을 개선하도록 함
- 이러한 MoCov3를 ViT에 적용하기 위한 가장 간단한 방법은 기존의 CNN backbone을 ViT로 대체하는 것이지만 이는 훈련 과정의 instability를 유발함
> - contrastive learning 기반 self-supervsed learning은 batch size를 크게  하는 경우에 더 많은 negative samples를 활용할 수 있어 성능에 도움을 주지만 ViT를 사용하는 경우에는 오히려 instability를 유발하고 이로 인해 성능이 감소함
> - Learning rate이 작은 경우에 training이 더 stable하지만 underfitting되며, learning rate을 크게 설정하는 경우에는 training이 instable해지고 정확도도 낮아짐
> - 또한, AdamW와 LAMB 두 optimizer를 사용했을 때 LAMB optimizer가 learning rate에 더 민감하였고, 따라서 본 논문에서는 AdamW optimizer 사용
- Gradient magnitude를 조사해본 결과, gradient의 갑작스러운 변화(spike)는 네트워크의 첫 레이어에서 발생하며 이는 instability가 shallower layer에서 발생한다는 것을 의미
- 따라서, 간단한 해결책으로 patch projection layer를 freeze하고 훈련을 진행함 (즉, random patch projection 수행, stop-gradient로 구현 가능)
- Random patch projection은 학습을 stabilize하게 하고, 더 부드러운 학습 곡선을 보여줌
> - MoCov3 외의 다른 self-supervised methods + ViT에서도 효과적

## 실험결과
- ViT + MoCo v3는 ViT를 다양한 siamese self-supervised framework로 학습한 여러 모델들의 성능을 능가
- Masked patch prediction 기반의 iGPT나 ViT보다도 좋은 성능
- Supervised ViT의 경우에 일정 크기 이상으로 모델의 크기가 커지면 성능에 악영향을 주었는데, self-superivsed ViT에서는 매우 큰 ViT 모델을 사용한 경우에 특정 task에 대하여 supervised보다 성능이 뛰어났음

## 의견
- /