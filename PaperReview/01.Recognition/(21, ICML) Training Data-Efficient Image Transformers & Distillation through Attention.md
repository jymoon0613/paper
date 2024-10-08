# Training Data-Efficient Image Transformers & Distillation through Attention (ICML, 2021)

[논문 링크](https://proceedings.mlr.press/v139/touvron21a)

<p align="center">
    <img width="400" alt='fig1' src="../img/touvron2021training.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Vision Transformers (ViT)는 좋은 성능을 보여주지만, 이를 위해서는 매우 많은 양의 학습 데이터와 컴퓨팅 자원이 필요함
- 본 논문에서는 이러한 문제를 극복하기 위해 ImageNet 훈련 데이터셋만을 학습에 사용하고, 비슷한 파라미터 수의 CNN과 유사한 학습 속도로 ViT를 훈련시키는 방법을 제안함 (Data-Efficient Image Transformers (DeIT))
- 또한, class token과 같은 역할을 하는 distillation token을 사용하여 ViT를 위한 새로운 knowledge distillation 방법을 제안함 

## 접근법
### (1) ViT
- ViT는 입력 이미지를 일정 크기의 패치로 나누어서 처리함
- Class token은 학습 가능한 벡터로서 시작 레이어에서 이미지 패치와 함께 concat되어 전체 레이어를 거쳐 처리됨
- Class token은 최종 단계에서 linear layer를 거쳐 class 예측을 위해 사용됨
- 이러한 class token은 self-attention이 class token과 이미지 패치 간의 정보를 인코딩하도록 강제함

### (2) Distillation through Attention
- Student ViT를 훈련시키기 위해 CNN 혹은 Transformer 계열의 strong image classifier를 teacher로 사용할 수 있다고 가정함
- ViT의 초기 임베딩 시, class token과 마찬가지로 trainable한 vector인 distillation token을 추가함
- Distillation을 위한 전체 loss 구성에서, student 모델은 class token을 통해 예측한 예측값과 ground truth label 간의 loss를 계산하여 기본적인 분류 loss를 구성하고, distillation token을 통해 예측한 예측값과 teacher 모델의 예측값 사이의 distillation 적용 결과를 distillation loss로 구성함

## 실험결과
- Teacher 모델로 Transformers가 아닌 CNN을 사용했을 때 성능이 더욱 좋았음
- 이는 student Transformer가 distillation을 통해 CNN의 inductive bias를 학습할 수 있기 때문임
- Hard distillation을 사용하고 distillation token을 사용했을때 accuracy가 가장 높음
- 다른 task의 dataset에서도 충분히 competitive한 성능을 보여줌
- DeiT는 ViT와 약간 다른 하이퍼파라미터와 augmentation 기법을 적용
- Transformer 기반 모델들은 많은 양의 데이터가 필요하기 때문에 extensive한 data augmentation이 필요
- 배치안에 있는 모두 다른 이미지들을 한번만 augmentation하지 말고 현재 배치사이즈에 있는 이미지에 대해 여러번 augmentation 해서 사용하면 모델을 더 잘 일반화 시킬 수 있다고 해당 논문에서 주장 (ex. 배치사이즈가 16일때 이미지를 4번씩 augmentation을 하게되면 이때 배치사이즈는 16 x 4 = 64)
- Repeated Augmentation은 기존 배치사이즈가 64일때 independent한 이미지 64장을 16장으로 줄이고 그것을 장당 4번씩 augmentation 함

## 의견
- ViT 모델을 knowledge distillation으로 훈련시켰을 때 훨씬 적은 데이터로도 더 높은 성능 달성