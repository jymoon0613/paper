# Adversarial Masking for Self-Supervised Learning (ICML, 2022)

[논문링크](https://proceedings.mlr.press/v162/shi22d.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/shi2022adversarial.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MLM에서 masking된 words는 전체 문장의 semantics를 반영하고 있지만, MAE, BEiT 등에서 사용하는 random masking은 이러한 semantics를 고려하지 않음
- 이러한 pixel masking과 word masking의 차이를 좁히기 위해 이미지에서 전체 object를 마스킹하는 방법을 사용할 수 있음
> - MAE의 high masking ratio도 이러한 맥락
- 하지만, 저자들은 얼마나 많은 patches를 마스킹하는지가 아니라 어떤 patches가 마스킹되는지가 표현 학습 측면에서 더 중요하다고 주장함
- 따라서, 본 논문에서는 adversarial objective를 기반으로한 masking strategy를 제안함 (adversarial inference-occlusion self-supervision, ADIOS)
> - ADIOS는 이미지에서 상관성이 큰 pixels를 식별하고 마스킹함
- ADIOS는 pixel-level reconstruction을 수행하지 않고 original image와 masked image의 representation distance를 최소화하는 방식으로 훈련함

## 접근법
- ADIOS는 inference model과 occlusion model로 구성됨
- Occlusion model은 입력 이미지로부터 마스크를 생성함
- Inference model은 원본 입력 이미지와 마스킹된 이미지를 입력으로 받아 각각의 representations를 출력함
- Inference model과 occlusion model은 adversarial한 방식으로 훈련됨
> - Occlusion model은 objective function이 최대화되도록, inference model은 objective function이 최소화되도록 훈련됨
- Inference model의 SSL framework는 SimCLR와 유사함
> - 하나의 이미지로부터 생성된 두 개의 augmented views에 대해 원본 이미지와 마스킹된 이미지의 representations를 출력
> - SimCLR의 loss function을 변형
- Occlusion model로는 U-Net을 사용함
> - 입력 이미지 크기와 동일한 크기의 mask를 output함
- 이때 occlusion model은 N개의 masks를 생성하며, 각 masks에 대해 inference model을 적용시킴

## 실험결과
- Linear probing, transfer learning 실험에서 좋은 성능 기록
- ADIOS는 semantically-meaningful한 masks를 생성함
- Random masking을 사용했을 때보다 성능이 좋았음

## 의견
- /