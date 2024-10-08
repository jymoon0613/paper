# Language-conditioned Detection Transformer (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2311.17902)

<p align="center">
    <img width="800" alt='fig1' src="../img/cho2024language.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 기존의 object detectors는 새로운 category를 예측하기 위해 다시 fine-tuning시켜야 하는 한계가 있었음
> - 매번 새로운 category에 대한 충분한 데이터를 구축하는 것은 scalable하지 않음
- Open-vocabulary detection은 vision-language models를 활용하여 어떠한 임의의 category도 detection할 수 있는 해결방안을 제시함
- 하지만 현재의 open-vocabulary detectors는 기존의 object detectors를 그대로 재사용하며, classification layer나 box feature fusion layer만을 text representation으로 대체하는 경향이 있음
> - 따라서 detector의 내부적인 작동 과정은 바뀌지 않음
- 본 논문에서는 language로 표현된 임의의 concept 집합에 따라 내부 작동 과정을 조정하는 Transformer 기반의 object detector를 제안함 (DECOLA)
- 특히 DECOLA는 최근의 weakly-labeled 데이터를 사용하는 환경에서 유용하게 사용될 수 있음
> - Weakly-labeled 데이터는 인터넷에서 수집된 이미지 데이터와 같이, 이미지와 함께 간단한 text나 tag가 주어지는 데이터를 의미함
> - DECOLA는 weaky-labeled 데이터의 image-level tags나 text descriptions로부터 pseudo-labels의 형태로 conditioned predictions 수행함

## 접근법
- 최신 DETR은 R-CNN과 비슷한 2-stage 구조를 보임
> - Transformer encoder로 image-level features를 추출함
> - 추출된 feature map의 각 grid position $x_{i,j}$에 대해 objectness scores를 계산
> - Top-k 개의 regions만이 object queries로서 다음 stage로 전달됨
> - 이후 decoding 과정에서는 각 object querie에 대해 모든 $C$개의 class probabilities와 bbox를 예측함
- DECOLA는 object queries에 language embedding를 반영하도록 함
> - Objectness score를 region feature $x_{i,j}$와 category name의 text representation $t(y)$ 간의 similarity로 정의함
> - 이후 decoding 과정에서는 각 object query에 대해 할당된 class $y$에 대한 단일 scalar(class probability)와 bbox를 예측함
> - 이러한 과정을 통해 detection pipeline에 semantic knowledge를 주입할 수 있음
- 이러한 detection 방식은 매우 정확한 예측이 가능하도록 하면서 DECOLA가 weakly-labeled 데이터에 대한 pseudo-labeler로서 기능할 수 있도록 함
> - Weakly-labeled 데이터의 image tags와 captions를 class label $y$로 사용하여 대규모 weakly-labeled 데이터로부터 pseudo-label(annotation)을 생성함 (offline pseudo-labeling)
- DECOLA를 open-vocabulary detector로 훈련시키기 위해 'an object'라는 text input과 함께 classifier에 class information을 주입함
> - Offline으로 생성한 pseudo-labeled 데이터와 기존 데이터셋을 모두 사용하여 학습시킴
> - 'an object'에 대한 text representation을 추출하고 region feature $x_{i,j}$와의 feature similarity를 계산하여 object queries를 추출
> - 기존 최신 DETR과 마찬가지로 decoding을 통해 각 object querie에 대해 모든 $\hat{C}$개의 class probabilities와 bbox를 예측함

## 실험결과
- LVIS 데이터셋에서 평가했을 때 open-vocabulary detection 환경에서 다른 베이스라인보다 더 좋은 성능 기록

## 의견
- /