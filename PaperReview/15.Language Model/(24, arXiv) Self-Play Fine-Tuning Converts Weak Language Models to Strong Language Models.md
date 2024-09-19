# Self-Play Fine-Tuning Converts Weak Language Models to Strong Language Models (arXiv, 2024).md

[논문링크](https://arxiv.org/abs/2401.01335)

<p align="center">
    <img width="600" alt='fig1' src="../img/chen2024self.png?raw=true"></br>
    <em><font size=2>Example of ground truth completion compared to the fine-tuned model generation at iteration 0 and 1.</font></em>
</p>

## 연구목적
- Supervised fine-tuning(SFT)와 Reinforcement Learning from Human Feedback (RLHF)은 LLM의 대표적인 post-pretraining 방법 중 하나이지만 많은 양의 human annotated data가 필요하다는 한계가 있음
- 본 논문에서는 self-play mechanisms을 활용한 효율적인 fine-tuning 기법인 Self-Play fIne-tuNing(SPIN)을 제안함
> - SPIN은 추가적인 human annotation 없이 기본적인 SFT 데이터셋만 사용함

## 접근법
- 한 쪽은 LLM과 human에 의해 생성된 답변을 구분하고, 다른 한 쪽은 human의 답변과 구분이 안되는 그럴듯한 답변을 생성하는 two-player game을 설정함
> - 두 player는 서로 다른 iteration동안 학습된 같은 LLM임
- Opponent LLM($t-1$)은 답변을 생성하고, main LLM($t$)는 답변을 구분함
> - $t$ 시점의 main LLM을 학습시키고, $t$ 시점의 main player는 $t+1$ 시점의 oppenent LLM이 됨 
- 이는 GAN의 adversarial training 방식과 유사함 
- SPIN의 loss function의 global optimum이 존재한다는 것을 이론적으로 증명함

## 실험결과
- SFT만 사용하여 fully fine-tuned된 모델에 SPIN을 추가하면 성능 향상이 더 가능함
- 또한, SPIN으로 훈련된 LLM은 추가적인 human data나 AI feedback을 사용한 LLM보다 여러 벤치마크에서 더 좋은 성능을 기록함
- 즉, SPIN은 self-play 과정을 통해 LLM의 self-evaluation 과정을 촉진하고, SFT나 RLHF와 같이 추가적인 human data를 요구하지 않음

## 의견
- /