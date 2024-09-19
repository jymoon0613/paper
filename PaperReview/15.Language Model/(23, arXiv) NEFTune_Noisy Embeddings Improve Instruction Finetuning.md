# NEFTune: Noisy Embeddings Improve Instruction Finetuning (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2310.05914)

<p align="center">
    <img width="600" alt='fig1' src="../img/jain2023neftune.png?raw=true"></br>
    <em><font size=2>AlpacaEval Win Rate percentage for LLaMA-2-7B models finetuned on various
datasets with and without NEFTune.</font></em>
</p>

## 연구목적
- LLM은 주로 rwa web data에서 훈련된 후 curated instruction data로 fine-tuning됨
- LLM의 성능은 주로 어떤 instruction 데이터셋으로 어떻게 훈련되는가에 좌우됨
- 본 논문에서는 instruction fine-tuning 과정에서 embedding vectors에 random noise를 추가하는 간단한 trick으로 LLM의 성능을 크게 향상시킬 수 있다는 것을 발견함 (Noisy
Embedding Instruction Fine Tuning, NEFTune)

## 접근법 및 실험결과
- NEFTune은 기본적인 instruction fine-tuning을 수행하는 동시에 샘플링된 instruction data에 uniform noise를 추가한 embedding vectors를 사용하여 fine-tuning을 수행함
- LLM으로는 LLaMA-1, LLaMA-2, and OPT-6.7B을 사용하였고, instruction 데이터셋으로는 Alpaca, Evol-Instruct, Open-Platypus, ShareGPT을 사용하였으며, 평가 프로토콜은 AlpacaEval과 Hugging Face OpenLLM Leaderboard를 활용
- 실험 결과, NEFTune은 conversational ability와 answer quality를 극적으로 향상시키는 효과가 있었음
- 이는 random noise를 추가하는 것이 특정 instruction-tuning 데이터셋에 대한 overfitting을 방지하기 때문으로 보임 (i.e., formatting details, exact wording, text length)
> - 특정 instruction dataset을 따르기보다는 pre-trained된 base model의 knowledge를 더 효과적으로 활용
> - Overfitting 문제를 실험적으로 검증
- 단, 단점으로는 기존보다 길고 복잡한 답변을 내놓는다는 점이 있음
> - 성능적으로는 큰 문제가 없음
> - 긴 답변이 추가적인 details를 제공하는 효과도 있었음
- NEFTune의 결과가 의미하는 바는 LLM 훈련 및 scaling-up 과정에서 regularization에 대한 고려가 필요하다는 것임
> - Regularization과 overfitting에 대한 연구가 지속적으로 이어져온 computer vision 분야와 달리 LLM community는 획일화된 training loops를 고집하는 경향이 있음
> - Optimization에 대한 고려에 치중되어 있으며 generalization에 대해서는 크게 고려하지 않음

## 의견
- /