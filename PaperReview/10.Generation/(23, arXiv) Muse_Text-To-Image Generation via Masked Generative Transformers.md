# Muse: Text-To-Image Generation via Masked Generative Transformers (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2301.00704)

<p align="center">
    <img width="600" alt='fig1' src="../img/chang2023muse.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 masked image modeling(MIM)을 사용하는 text-to-image generation 모델을 제안함 (Muse)
> - Quantized image tokens와 parallel decoding을 사용하여 기존의 모델들보다 훨씬 빠름

## 접근법
- 우선 입력 text caption을 pre-train된 T5-XXL 인코더로 입력하여 text embedding vectors를 추출함
- 입력 이미지는 VQGAN 모델을 통해 semantic tokens로 매핑됨
> - 256x256 resolution을 사용하는 base 모델과 512x512 resolution을 사용하는 super-res 모델을 사용함
- Text tokens는 그대로 유지한채 image tokens의 일부를 마스킹하고 MASK tokens로 대체함
- Base 모델은 visible image tokens와 text tokens를 Transformer 구조에 입력하여 masked image tokens를 예측하도록 훈련됨
- Super-res 모델은 base 모델의 output token map과 text input을 사용하여 masked image tokens를 예측함
> - Base 모델의 output token map과 text input을 concat하여 cross-attention의 key, value로 사용함
> - Base 모델을 먼저 훈련시킨 뒤 super-res 모델을 훈련시킴
- Base 모델과 super-res 모델 모두 parallel decoding을 사용하여 짧은 decoding step으로도 inference 수행 가능

## 실험결과
- Muse는 Imagen, Dall-E2와 비교했을 때 더 퀄리티있는 이미지를 생성했음
- Muse-632M 모델은 CC3M 데이터셋에서 FID score, CLIP score 모두 SOTA를 기록
> - FID score는 생성된 샘플의 퀄리티와 다양성을 측정
> - CLIP socre는 image/text alignment를 측정
- Human evaluation을 진행했을 때 Muse는 70.6%의 prompts에서 Stable Diffusion 보다 더 잘 align한다고 선택되었음
> - Text prompts와 더 잘 매칭되는 생성 이미지를 선택

## 의견
- /