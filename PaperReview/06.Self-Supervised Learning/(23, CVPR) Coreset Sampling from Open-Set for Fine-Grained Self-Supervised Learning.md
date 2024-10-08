# Coreset Sampling from Open-Set for Fine-Grained Self-Supervised Learning (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2303.11101)

<p align="center">
    <img width="600" alt='fig1' src="../img/kim2023coreset.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-grained visual recognition(FGVR)은 두 가지 어려움을 수반함
> - Annotation 과정에서 상당한 전문 지식이 필요하므로 annotation 비용이 크며, 이는 labeled 데이터 부족으로 이어짐
> - 여러 fine-grained datasets 및 tasks에 범용적으로 사용할 수 있는 pre-trained model에 대한 요구가 큼
- 최근 self-supervised learning(SSL)이 annotations 없이 여러 downstream tasks에 적용가능한 표현 학습을 위해 사용되고 있음
- SSL은 large-scale unlabeled set 혹은 open-set을 pre-training을 위해 사용함
- 본 논문에서는, fine-grained dataset 뿐만 아니라 open-set을 사용하는 Open-Set Self-Supervised Learning(OpenSSL)을 제안함
- Open-set을 사용하는 경우, open-set에는 fine-grained dataset과 무관한 샘플들이 존재할 것이기 때문에 distribution mismatch를 고려해야함
- 이러한 mismatch problem을 해결하기 위해 target dataset과 유사한 semantics를 보이는 coreset(open-set의 부분집합)을 찾는 것이 중요함
> - 이를 검증하기 위해, ImageNet을 open-set으로 두고, 여러 fine-grained 데이터셋(target dataset)에 대해 도움이 될만한 class samples를 임의로 선정하여 coreset으로서 SSL에 사용한 결과, 랜덤하게 coreset을 샘플링하여 사용하거나, 모든 open-set을 다 사용하는 경우보다 성능이 좋았음
- 따라서, 본 논문은 unlabeled open-set으로부터 효과적인 coreset을 샘플링하는 SimCore 방법을 제안함

## 접근법
- Contrastive learning 기반의 SSL을 가정함
- SimCore는 open-set에서 target dataset과 가장 유사한 subset을 찾기 위해 제안됨
> - target dataset으로 약간 훈련된 모델을 사용
> - Target dataset과 open-set으로부터 임의로 나누어진 subset의 representation을 구하고, 두 dataset의 representation의 유사도(dot product)가 최대가 되는 subset을 coreset으로 선정함
- 이때 연산량을 줄이기 위해 target dataset의 모든 샘플을 사용하는 것이 아닌 target dataset에 clustering을 적용한 뒤, cluster ceters를 대체하여 사용함
- 이러한 selection round를 여러 번 반복하여 최종 all corset samples를 얻음
- 이때 t번째 round에서 샘플링된 coreset이 첫번째 coreset만큼 target dataset에 대한 유사도를 보이지 않으면 반복을 종료시킴 (threshold값 기반)

## 실험결과
- 11개의 FGVR 데이터셋에서 검증함
- Linear probing에서, SimCore를 통해 coreset을 선정하는 경우, open-set에서 랜덤하게 coreset을 설정하는 경우보다 항상 성능이 좋았음(효과적인 coreset 선정이 중요)
- Open-set에서 coreset으로 사용되는 비율(budget)을 얼마로 설정하는지는 각 데이터셋에서의 학습 성능에 큰 영향을 주었고, 따라서 round 반복을 통해 adaptive하게 사이즈를 결정하는 작업은 성능 향상에 긍정적 효과를 주었음
- Backbone(i.e. ResNet101, EfficientNet) 및 contrastive learning method(i.e. BYOL, SimCLR)를 변경해도 SimCore는 일관적인 성능 향상을 가져옴
- Open-set으로 ImageNet 대신 MS COCO, iNaturalist 2021-mini, Places365을 사용했을 때도 유의미한 성능 향상이 있었음
> - 각 target dataset마다 어떤 데이터셋을 open-set으로 사용하는 지에 따른 성능 차이가 존재했음
- 인터넷 상의 uncurated dataset을 open-set으로 사용했을 때도 긍정적인 효과가 있었음
- Feature representation을 시각화한 결과, SimCore는 실제로 target dataset과 유사한 open-set samples를 coreset으로 샘플링하고 있었음

## 의견
- /