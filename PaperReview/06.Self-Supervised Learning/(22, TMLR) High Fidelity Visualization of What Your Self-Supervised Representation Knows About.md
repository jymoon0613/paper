# High Fidelity Visualization of What Your Self-Supervised Representation Knows About (TMLR, 2022)

[논문링크](https://arxiv.org/abs/2112.09164)

<p align="center">
    <img width="700" alt='fig1' src="../img/bordes2021high.png?raw=true"></br>
    <em><font size=2>Representations of the real picture of Earth on the left (source: NASA) as conditioning for RCDM.</font></em>
</p>

## 연구목적
- 본 논문에서는 generative models를 통해 self-supervised learning(SSL)으로 훈련된 모델의 representations를 시각화하고자 함(Representation-Conditionned Diffusion Model, RCDM)
> - 모델이 SSL로부터 무엇을 배우는지 탐구하는 시각화 방법 설계

## 접근법 및 실험결과
- RCDM은 Ablated Diffusion Model(ADM)을 baseline으로 사용함
- Backbone은 ImageNet에서 DINO로 pre-trained된 ResNet-50을 사용함
- ADM의 Group Normalization layers를 (ResNet-50에서 추출한)representation $h$에 의해 condition되는 Conditional Batch Normalization layers로 변경함
- 이러한 conditional diffusion model은 original image와 매우 흡사한 이미지를 생성함
- DINO 외에도 SimCLR, Barlow Twins 등 여러 SSL로 훈련된 ResNet-50으로 conditioning하여 RCDM을 훈련시켜봄
> - 훈련된 backbone에서 추출한 representations로 conditioning했을 때는 SSL 방법 간 생성된 이미지의 차이가 거의 없었음
> - 반면, projector-level의 representations로 conditioning했을 때는 SSL 방법마다 생성된 이미지의 variance가 커짐
> - 실제로 backbone 자체의 linear probing 성능이 projection head를 포함하는 것보다 성능이 좋았음
> - 이는 backbone representation이 입력 이미지에 대한 더 많은 정보를 담고 있으며, linear classification에 적합함을 의미
- RCDM을 사용하여 SSL 방법마다 data augmentation에 얼마나 민감하게 representations가 반응하는지도 확인가능함
- 또한, SSL로 훈련된 representations는 background 정보와 object 정보를 서로 다른 dimension에 매핑함

## 의견
- /