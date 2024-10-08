# ClipCap: CLIP Prefix for Image Captioning (arXiv, 2021)

[논문링크](https://arxiv.org/abs/2111.09734)

<p align="center">
    <img width="600" alt='fig1' src="../img/mokady2021clipcap.png?raw=true"></br>
    <em><font size=2>Overview of our transformer-based architecture.</font></em>
</p>

## 연구목적
- Image captioning 모델의 대부분은 visual cues 모델링을 위한 encoder와 caption 생성을 위한 text decoder로 구성됨
- 하지만, 이러한 모델은 visual/text representations를 align하기 위해 긴 학습 시간과 많은 trainabel parameters, 방대한 데이터 양과 더 세분화된 annotations가 요구됨
- 이러한 resource-hungry한 특성은 모델이 새로운 도메인에 대해 자주 재훈련이 필요한 경우 등 실제 모델 적용에 있어서 큰 제약이 됨
- 본 논문에서는 vision-language pre-trainded 모델(CLIP)을 활용한 lightweight captioning model을 제안함
- CLIP은 image와 text 간의 shared representations를 갖도록 설계되어 image/text representations를 align하기 위한 resource가 상대적으로 덜 필요함

## 접근법
- 제안하는 방법은 우선 pre-trained CLIP visual encoder로부터 visual features를 추출함
- Visual features는 간단한 mapping network를 거쳐 prefix embeddings로 인코딩됨
- Prefix embeddings와 learnable caption tokens가 concat되어 GPT-2로 입력되고, GPT-2는 autoregressive하게 캡션을 생성함
- CLIP encoder는 고정된 채로 mapping network와 GPT-2를 학습시킴
  - 이 경우 mapping network는 간단한 MLP임
  - MLP의 입력은 CLIP encoder의 visual features임
- 혹은 trainable parameters를 더 줄이기 위해 GPT-2도 freeze시킬 수도 있음
  - 이 경우 mapping network는 Transformer 구조임
  - Transformer의 입력은 CLIP encoder의 visual features와 learnable caption tokens임

## 실험결과
- 매우 짧은 훈련 시간과 적은 trainable parameters에도 불구하고 기존 SOTA 모델들과 거의 비슷한 성능 기록
- 제안하는 방법은 여러 unseen images에 대해서도 준수한 캡션을 생성했음 (일반화가 잘 됨)
- GPT-2를 같이 fine-tuning하는 것은 더 expressive한 캡션을 생성하도록 하지만 overfitting에 더 취약하다는 문제가 있었음
- Prefix를 가장 가까운 word로 치환하여 표시해본 결과, GPT-2를 같이 fine-tuning할 때는 prefix가 interpretable하고 caption과 어느정도 유사함을 보인 반면, mapping network만 훈련시킬 때는 interpretability가 매우 떨어졌음
  - Prefix는 visual features로부터 word embeddings와 같은 차원을 공유하도록 매핑되므로, 주어진 prefix와 vocabulary tokens 간의 cosine similarity를 계산하여 가장 가까운 word를 찾을 수 있음

## 의견
- /