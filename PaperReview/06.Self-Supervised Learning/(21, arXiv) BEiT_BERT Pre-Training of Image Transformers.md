# BEiT: BERT Pre-Training of Image Transformers (arXiv, 2021)

[논문링크](https://arxiv.org/abs/2106.08254)

<p align="center">
    <img width="600" alt='fig1' src="../img/bao2021beit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- BERT의 masked language modeling(MLM)에서 영감을 받아 ViT를 pretrain하기 위한 denoising auto-encoding idea를 고안함
> - 모델이 masked patches의 raw pixels를 예측하도록 하는 regression problem을 설계
- 하지만, pixel-level recovery task는 모델이 short-range dependencies 및 high-frequency details를 모델링하는 것에 과하게 편향된다는 문제가 있음
- 본 논문에서는 BERT의 훈련 방식에서 착안한 pretraining task인 masked image modeling(MIM)을 제안하며, 해당 task로 훈련된 모델인 bidirectional encoder representation from image transformer(BEiT)를 제안함
> - 하나의 입력 이미지로부터 image patches와 visual tokens를 생성함
> - 이때 visual tokens는 discrete VAE의 latent codes로 구함
> - 매 iteration 마다 image patches의 일부가 랜덤하게 마스킹되어 Transformer로 입력됨
> - 이때 모델은 masked patches의 raw pixels가 아닌 원본 이미지의 visual tokens를 복원하도록 훈련됨

## 접근법
- 입력 이미지는 두 개의 representations로 표현됨: image patches, visual tokens
- Image patches는 ViT의 patch embedding을 통해 구할 수 있음
- Visual tokens는 natural language와 마찬가지로 image를 discrete한 tokens의 sequence로 표현한 것
> - Image tokenizer를 사용함
> - Discrete VAE(dVAE)로 학습한 image tokenizer를 사용함
- Backbone으로는 ViT와 마찬가지로 standard Transformer를 사용함
- MIM을 진행하기 위해 image patches의 일부가 마스킹된 후 Transformer로 입력됨 (40%)
> - Masked patches는 learnable embedding을 대체됨
- Transformer는 masked patches의 visual tokens를 예측함
> - Transformer의 마지막 output hidden vector에 softmax classifier를 적용
- Objective function은 predicted visual tokens와 correct visual tokens 간의 log-likelihood를 최대화하는 것
- 또한, image patches 마스킹 시 block-wise masking 전략을 사용함
> - Short-range connections을 제거하기 위해
> - 즉, 인접한 pixels 간에는 매우 강력한 상관관계가 존재한다는 visual domain의 locality 특성을 고려하여 인접한 patche들을 한꺼번에 제거함
- 인접하게 위치하고 있는 image patch block이 마스킹됨

## 실험결과
- 훈련된 BEiT를 fine-tuning하여 image classification 및 semantic segmentation tasks에서 평가함
- Classification task에서 supervised하게 훈련된 ViT보다 좋은 성능 달성
- 추가로, DINO, MoCov3, iGPT를 사용하여 self-supervised하게 훈련된 모델보다 좋은 성능 달성
- Semantic segmentation task에서 DINO로 훈련된 모델 및 supervised하게 pre-train된 모델보다 좋은 성능 달성 

## 의견
- /