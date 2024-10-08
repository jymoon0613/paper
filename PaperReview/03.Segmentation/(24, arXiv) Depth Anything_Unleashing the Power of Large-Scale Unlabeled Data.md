# Depth Anything: Unleashing the Power of Large-Scale Unlabeled Data (arXiv, 2024)

[논문링크](https://arxiv.org/abs/2401.10891)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2024depth.png?raw=true"></br>
    <em><font size=2>Overall pipeline.</font></em>
</p>

## 연구목적
- 본 논문에서는 어떠한 환경이나 이미지에 대해서도 high-quality depth information을 추출하는 Monocular Depth Estimation(MDE) foundation model을 제안함 (Depth Anything)
- Foundation model 훈련을 위해서는 대규모 데이터셋이 요구되며, 이를 위해 unlabeled images를 적극 활용함

## 접근법
- MiDaS를 baseline으로 사용함
- 우선 labeled images로 supervised하게 MDE task를 학습함
- 학습된 모델을 teacher로 사용하여 대규모의 unlabeled images로부터 pseudo labels를 생성하고, self-training 방식으로 같은 구조의 student를 학습시킴
- 이때 student가 unlabeled images로부터 더 많은 visual knowledge를 학습하도록 optimization target을 강화함
> - Unlabeled images에 strong color distortions와 CutMix augmentations를 추가함
- 또한, student의 semantic knowledge를 향상시키기 위해 feature alignment를 auxiliary task로 추가함
> - DINOv2가 image retrieval과 같은 semantic-related tasks에 강점이 있다는 것을 이용하여, unlabeled images에 대한 student의 feature가 freeze된 DINOv2의 feature와 유사해지도록 학습함

## 실험결과
- Zero-shot MDE setting에서 기존 baseline과 유사하거나 더 좋은 성능을 기록함
- 시각화 결과, DAN은 다양한 도메인에서 robust한 MDE 성능을 보여주었음

## 의견
- /