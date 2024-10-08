# LLaMA: Open and Efficient Foundation Language Models (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2302.13971)

<p align="center">
    <img width="400" alt='fig1' src="../img/touvron2023llama.png?raw=true"></br>
    <em><font size=2>Zero-shot performance on Common Sense Reasoning tasks.</font></em>
</p>

## 연구목적
- 파라미터 수를 증가시키면 증가시킬수록 성능이 더 좋아질 것이라는 기대에서 LLM을 scale-up하려는 시도가 계속되어 왔음
- 하지만 최근의 연구는 주어진 computation budget 하에서 가장 좋은 성능을 내는 모델은 가장 큰 모델이 아니며 훨씬 작은 모델이 더 많은 데이터로 훈련되었을 때라는 것을 보여줌
- Scaling laws의 목적은 특정 training computed budget에 대한 데이터셋/모델 크기를 가장 잘 scale하는 방법을 결정하는 것임
  - Inference budget은 고려하지 않는 측면이 있음
- Large model이 간단한 훈련으로 일정 수준의 성능에 도달하는 데는 더 쉬울 수 있으나, 같은 성능을 위해 더 작은 모델을 길게 훈련시키는 것이 inference 측면에서 더 유리할 수 있음
- 본 논문에서는 여러 inference budgets 수준에서 가장 성능이 좋은 LLM series를 도출함 (LLaMA)
  - LLM series는 7B - 65B에 이르는 모델 파라미터 규모를 가짐

## 접근법
- LLaMA는 open sourcing을 위해 publicly available한 데이터셋만 훈련에 사용함
  - e.g., Wikipedia, GitHub, arXiv
- 모델 구조는 Transformer 아키텍처를 기반으로 일부 수정을 함
  - e.g., pre-normalization(GPT3), SwiGLU activation function(PaLM), Rotary Embeddings(GPTNeo)

## 실험결과
- LLaMA-13B는 대부분의 벤치마크에서 GPT-3의 성능을 능가함
- LLaMA-65B는 PaLM-540B와 유사한 성능을 보임

## 의견
- /