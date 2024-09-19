# Improving Visual Representation Learning through Perceptual Understanding (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2212.14504)

<p align="center">
    <img width="800" alt='fig1' src="../img/tukra2023improving.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MAE는 모델이 매우 높은 비율로 마스킹된 이미지를 reconstruction하도록 훈련시킴으로써 모델이 제한된 정보로부터 high-level context를 추론하도록 함
> - 즉, MAE는 높은 마스킹 비율이라는 '암묵적인' 조건을 통해 모델이 high-level semantic을 학습하도록 함
- 본 논문에서는 이러한 MAE를 기반으로 하되 high-level semantic features 학습을 learning objective에 직접적으로(명시적으로) 통합시키고자 함
> - With perceptual loss, adversarial learning
> - 기존 MAE의 loss function에 perceptual loss와 adversarial loss를 추가

## 접근법
- 먼저, 모델이 scene-level information을 고려하여 perceptual reconstruction quality를 개선시키도록 perceptual loss를 MAE loss function에 추가함
- Feature matching 기반의 perceptual loss를 정의함
> - Feature matching은 사전에 정의된 pre-trained network(e.g. VGG)를 사용하여 network의 각 레이어에서 실제 이미지 features와 생성 이미지의 features 유사도를 계산하여 generator의 loss에 추가하는 방법임
> - 이는 generator의 생성 퀄리티를 boost할 수 있음
> - 본 논문에서는 discriminator를 pre-trained network 대신 사용함
> - Discriminator의 각 layer에서 MAE가 reconstruction한 이미지와 원본 이미지의 feature map의 유사도를 계산하고 이를 MAE의 loss function에 추가함
- 또한 discriminator의 학습을 위해 adversarial loss(LS-GAN)가 MAE loss function에 추가됨
- MSG-GAN의 개념을 확장하여 enocer와 decoder의 각 layer 간의 skip-connection을 설정함

## 실험결과
- Reconstruction quality가 상당히 개선됨
- Linear probing 성능에서 MAE를 능가함
- Classification, detection, segmentation에서 모두 좋은 성능을 보임

## 의견
- /