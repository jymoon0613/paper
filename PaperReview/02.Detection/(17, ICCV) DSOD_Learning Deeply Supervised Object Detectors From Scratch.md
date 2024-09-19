# DSOD: Learning Deeply Supervised Object Detectors From Scratch (ICCV, 2017)

[논문링크](https://openaccess.thecvf.com/content_iccv_2017/html/Shen_DSOD_Learning_Deeply_ICCV_2017_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/shen2017dsod.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Detection 모델들은 주로 ImageNet classification tasks에서 pre-trained되지만 이는 다음과 같은 문제들을 야기할 수 있음:
> - Pre-trained 모델들은 classification task에 최적화되어 설계되었으며, 주로 많은 parameters를 지니고 있기 때문에 structure design space가 제한적이고 computing resources가 많이 필요함
> - Classification과 detection의 loss functions와 category distributions가 상이하기 때문에 learning bias가 발생하기 쉬우며 이는 suboptimal한 detection 성능으로 이어짐
> - Fine-tuning 기술의 발전이 이러한 gap을 어느정도 완화시켜주지만 여전히 두 tasks 사이의 domain mismatch가 존재함
- 본 논문에서는 pre-trained 없이 detection task에서 scratch로 학습하며, resource efficient한 detection 모델인 deeply supervised objection detectors(DSOD)를 제안함

## 접근법
- DSOD는 대표적인 one-stage detector인 SSD를 베이스라인으로 사용하며 두 parts로 나뉘어짐:
> - Feature 추출을 위한 backbone sub-network
> - Multi-scale feature maps로부터 detection을 수행하는 front-end sub-network
- Backbone sub-network는 DenseNet 구조에 기반하여 정의함
- SSD, YOLO와 마찬가지로 DSOD는 detection을 regression으로 치환하는 proposal-free methods임
> - 선행 실험으로부터 two-stage detectors는 pre-trained 모델을 사용하지 않으면 성능이 매우 안좋다는 것을 발견함
> - 저자들은 그 이유가 two-stage detectors에서 주로 사용되는 RoI pooling이 gradients의 흐름을 방해하기 때문이라고 추측함
- 또한, DSOD는 deep supervision 방식으로 훈련됨
> - Deep supervision: 깊은 네트워크를 더 효과적으로 훈련시키기 위해 네트워크 사이사이에 auxiliary objective functions를 추가하여 학습시키는 방식으로, vanishing gradients problem을 완화시키 효과가 있음
> - DSOD는 auxiliary objective functions를 추가하는 대신 dense layer-wise connection을 사용하는 우회적인 방식으로 deep supervision을 적용함
> - 하위 layers는 dense skip-connection pathway를 통해 supervised signals를 추가적으로 전달받을 수 있음

## 실험결과
- Pre-trained 모델을 사용하지 않고도 pre-trained SSD, YOLO, Faster R-CNN 등보다 뛰어나거나 비슷한 성능을 기록함

## 의견
- / 