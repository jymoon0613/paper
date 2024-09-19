# ViTPose: Simple Vision Transformer Baselines for Human Pose Estimation (NIPS, 2022)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2022/hash/fbb10d319d44f8c3b4720873e4177c65-Abstract-Conference.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/xu2022vitpose.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 plain ViT 아키텍처로 pose estimation task를 수행함(ViTPose)
> - 높은 성능뿐만 아니라 simplicity, scalability, flexibility, transferability 측면에서도 뛰어남

## 접근법
- (1) 먼저, ViTPose는 구조적으로 매우 간단함(simplicity)
- 입력 이미지는 patch embedding 이후 ViT encoder layers를 통과함
- 이후 처리된 feature map은 lightweight decoder layers를 통과하여 최종 예측이 수행됨
> - Decoder layer는 deconvolutional layer, batch normalization, ReLU activation으로 구성됨
> - 1x1 convolution을 적용하여 최종적으로 keypoints에 대한 localization heatmaps 예측 수행
- (2) ViTPose의 구조적 간단함으로 인해 모델 사이즈를 조절하는 것이 쉬움(scalability)
> - Transformer layers를 더 쌓거나 feature dimensions를 중가시킴
> - ViT-B/L/H/G를 모두 실험해봤을 때 모델 사이즈가 커질수록 성능이 일관적으로 증가했음
- (3) ViTPose는 매우 유연함(flexibility)
- 먼저, pre-training data에 대해 유연함
> - ImageNet pre-training 대신 MS COCO의 images로 MAE를 수행했을 때 pre-training data가 매우 적음(ImageNet의 절반 이하)에도 불구하고 ImageNet pre-trained와 거의 비슷한 성능 달성
- 또한, ViTPose는 resolution에도 유연하여 input resolution 혹은 feature resolution이 커질수록 더 높은 성능을 보여줌
- Attention type에도 유연함을 보임
> - High-resolution에 self-attention을 적용하면 computational cost가 매우 커짐
> - 이를 해결하기 위해 window-based attention이나 pooling attention을 적용해도 기존 self-attention과 거의 비슷한 성능 달성
- Fine-tuning 방식에도 유연함을 보임
> - Pre-trained된 ViTPose에서 FFN 혹은 MHSA만 fine-tuning해도 fully fine-tuning과 거의 비슷한 성능 달성
- 여러 tasks에 있어서도 유연함을 보임
> - Shared ViT backbone을 두고 multiple decoders를 정의하여 multi-dataset training을 진행할 수 있음
> - COCO, AIC, MPII 데이터셋으로 multi-dataset training을 수행하면 COCO val set에서의 test 성능이 유의미하게 향상됨
- (4) ViTPose는 transferability 측면에서도 유리함
> - 간단한 learnable token을 추가하여 knowledge distillation으로 small 모델을 효과적으로 훈련 가능

## 실험결과
- Top-down 방식의 구현을 사용: detector가 person instances를 detect하고 ViTPose가 detected person instances에 적용되어 keypoints를 예측
- ViTPose는 MS COCO val set 및 test set에서 SOTA 달성

## 의견
- /