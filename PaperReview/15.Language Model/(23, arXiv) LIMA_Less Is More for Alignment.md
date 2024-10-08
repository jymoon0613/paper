# LIMA: Less Is More for Alignment (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2305.11206)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhou2024lima.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- LLM은 pre-train 이후 적용하고자 하는 tasks에 align됨
> - 거대한 multi-million-example datasets을 사용한 instruction tuning 방식
> - 수백만의 human annotatios로부터 수집된 human feedback을 사용하는 reinforcement learning 방식
- 하지만, 이러한 alignment methods는 ChatGPT 수준의 성능을 달성하기 위해 많은 연산량과 특수 데이터가 필요함
- 본 연구에서는 잘 사전 학습된 모델에 1000개의 잘 엄선된 labeled data로 fine-tuning 한다면 좋은 성능을 낼 수 있음을 주장
> - Alignment는 LLM이 pre-training 과정에서 이미 학습한 knowledge와 capabilities를 표출하기 위해 유저와 상호작용하는 스타일 혹은 포맷을 학습하는 간단한 과정에 불과
> - Pre-train된 65B-parameter LLaMa 모델을 엄선된 1000개의 labeled data로 fine-tuning
> - 1000개의 labeled data는 Stack Exchange와 wikiHow에서 상위로 기록된 750개의 질문/답변 데이터와 연구자들이 직접 작성한 프롬프트/응답 데이터 250개로 구성

## 접근법
- 본 논문에서는 Superficial Alignment Hypothesis를 정의
> - LLM의 knowledges와 capabilities는 pre-training 과정에서 거의 모두 학습되며, alignment는 학습한 내용으로 유저와 상호작용하는 포맷을 학습하는 것 뿐임
- 1000개의 prompts/responses 쌍을 데이터셋으로 사용함
> - Output(responses)은 형식적으로 유사하지만 input(prompts)은 다양한 주제를 다룰 수 있도록 구성
> - Output의 형식은 AI assistant 스타일을 목표로 함
- Stack Exchange, wikiHow, Pushshift Reddit Dataset에서 데이터 수집
> - Stack Exchange, wikiHow의 answers는 상당히 align되어 있기에 그대로 사용하였지만, Reddit에서 수집된 answers는 도움이 되지 않는 답변(단순 유머)도 있기에 적절한 답변만 정제하여 사용
> - 데이터의 퀄리티와 적절한 output 형식을 학습할 수 있도록 필터링 및 문장 전처리 과정 수행
- 데이터의 다양화를 위해, 온라인에서 수집된 데이터셋 외에 연구진들이 직접 제작한 prompts도 학습에 사용

## 실험결과
- SOTA LLM과의 성능 비교 수행: Alpaca 65B, DaVinci003, Bard, Claude, GPT-4
- Human evaluation으로 평가 
> - Human preferences: 사람에게 두 모델에서 나온 답변을 보여준 뒤 어느 답변이 더 나은지 판단
- LIMA는 OpenAI의 RLHF로 학습한 DaVinci003보다 성능이 좋았으며 52000개의 데이터로 학습한 Alpaca 65B 모델보다 성능이 좋았음
- Fine-tuning에서 상당히 적은 데이터로 학습시키는 것만으로도 충분함
> - LLM의 사전 학습이 더 중요함
> - Fin-tuning 시 데이터의 양보다는 질이 더 중요함
> - 대규모 학습 데이터가 필요한 instruction tuning 혹은 reinforcement learning 역시 데이터의 퀄리티가 좋은 때 유의미함

## 의견
- /