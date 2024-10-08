# OverFeat: Integrated Recognition, Localization and Detection using Convolutional Networks (ICLR, 2014)

[논문링크](https://arxiv.org/abs/1312.6229)

<p align="center">
    <img width="700" alt='fig1' src="../img/sermanet2013overfeat.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 CNN이 classification, localization, detection 모두에 대해 동시에 학습된다면 세 tasks에서 모두 성능을 향상시킬 수 있다고 주장함
> - Classification, localization, detection을 모두 수행하는 single CNN 구조를 제안함 (OverFeat)

## 접근법
- 본 논문에서는 하나의 CNN 모델에 대해 (i) classification, (ii) localization, (iii) detection의 순서로 세 가지 vision tasks를 탐구함
> - Classification: 이미지에 존재하는 하나의 object, 하나의 ground-truth에 대해 5개의 클래스를 예측함 (top-5 accuracy)
> - Localization: classification과 마찬가지로 이미지에 존재하는 하나의 object, 하나의 ground-truth에 대해 5개의 클래스를 예측하되, 각 예측에 대한 bbox도 같이 예측해야 함 (ground-truth bbox와 IoU 0.5 이상)
> - Detection: classification, localization과 달리 이미지 내에 다수의 objects가 존재할 수 있으며, 방식은 localization과 동일
- OverFeat 모델은 AlexNet을 약간 수정하여 정의됨 
- 먼저, OverFeat을 classification task에 대해 훈련시킴
- 이때 네트워크의 성능을 높이기 위해 마지막 pooling operation을 offset 기반의 방식으로 수정함
> - Classifier의 viewing window를 약간씩 변경시켜 classifier가 마치 multiple views를 참조하는 것과 같은 효과를 줌
> - 하나의 feature map이 정의된 pooling layer를 통과하면 서로 다른 offset에 따라 pooling이 적용된 9개의 feature maps가 산출됨
> - 9개의 feature maps에 대해 각각 $C$개의 class 예측을 수행하고 aggregate함
- Localization 시 6개의 서로 다른 scale의 input images를 사용함
- 우선, 기존 classifier의 예측값은 각 location 별 confidence score를 의미함
> - 변형된 pooling 방식에 의해 9개의 feature maps가 생성되며, 생성된 각 feature maps는 주어진 offset setting에서 일정한 receptive fields를 표현하고 있음
> - 따라서, classifier는 9개의 feature maps에서 표현되는 각 receptive field 내에 특정 object가 포함될 확률을 계산하고 있는 것임
- 따라서, pooled feature maps에 fully-connected layers로 구성된 regression network만을 연결하여 각 location 별 bbox 좌표를 예측함
> - 이떄 ground truth box와 IoU가 0.5 미만인 예측 box는 학습에 포함시키지 않음
- Localization task를 훈련할 때는 feature extractor는 freeze시키고 regression network만 훈련시킴
- 모든 예측 값에 대해 greedy merge strategy를 적용하여 불필요한 box를 병합하고 최적의 bbox를 출력
- Detection은 localization과 매우 유사하나 차이점은 negative samples를 훈련 과정에서 고려한다는 것임
- Detection task에 대해 네트워크를 fine-tuning함

## 실험결과
- Overfeat 모델은 ImageNet2013 classification, localization, detection task에서 각각 4위, 1위, 1위를 차지함

## 의견
- / 