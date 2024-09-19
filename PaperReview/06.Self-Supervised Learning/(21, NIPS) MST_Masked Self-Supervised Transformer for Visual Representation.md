# MST: Masked Self-Supervised Transformer for Visual Representation (NIPS, 2021)

[논문링크](https://proceedings.neurips.cc/paper/2021/hash/6dbbe6abe5f14af882ff977fc3f35501-Abstract.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/li2021mst.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MIM 시 image patches를 랜덤하게 마스킹하는 경우 주어진 이미지의 crucial region을 표현하는 패치들이 마스킹될 수 있고, 이는 학습 모델의 성능 하락을 야기할 수 있음
- 본 논문에서는 crucial region의 tokens를 마스킹하는 것을 피하기 위해 multi-head self-attention map에 기반한 마스킹 전략을 제안함 (attention-guided masking strategy)
- 또한, masked tokens에 대한 예측만 수행하는 것은 모델이 local region에 과하게 편향되도록 할 수 있음
- 따라서, 본 논문에서는 global semantic information을 보존하면서 local context를 capture할 수 있는 새로운 MIM 기법인 masked self-supervised Transformer approach를 제안함(MST)

## 접근법
- 입력 이미지에 random data augmentation을 적용하여 서로 다른 views를 생성함
- 각 views는 teacher network와 student network로 입력됨
> - Teacher network와 student network는 같은 구조임
> - Teacher network의 파라미터는 student network의 파라미터의 moving-average로 업데이트됨
- Teacher network의 output과 student network의 output 간의 cross-entropy를 최소화하는 방식으로 훈련됨
- Attention-guided masking을 수행하기 위해 teacher network의 마지막 blcok의 attention map을 추출함
- 각 token을 (CLS token과의)attention score를 기준으로 정렬한 뒤 낮은 score를 기록한 patch부터 일부 마스킹함
> - Masking된 tokens는 learnable embeddings로 대체됨
- CNN 기반의 decoder를 디자인함
- 전체 이미지에 대해 pixel-level reconstruction loss를 정의

## 실험결과
- DINO, MoCov3보다 성능 좋았음
- Dense prediction tasks에서도 좋았음
- Random masking보다 attention-guided masking을 사용할 때 성능이 더 좋았음

## 의견
- /