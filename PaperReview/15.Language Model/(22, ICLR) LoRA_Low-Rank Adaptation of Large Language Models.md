# LoRA: Low-Rank Adaptation of Large Language Models (ICLR, 2022)

[논문링크](https://arxiv.org/abs/2106.09685)

<p align="center">
    <img width="300" alt='fig1' src="../img/hu2021lora.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 대부분의 NLP applications는 pre-trained LLM을 fine-tuning해서 사용하지만, pre-trained 모델의 모든 parameters를 tuning한다는 점에서 부담이 있음
- 이러한 문제를 해결하기 위해 parameters의 일부만 사용하거나 새로운 task를 위한 외부 모듈을 학습하는 방법들이 고안되었음
- 하지만, 이러한 기존의 기법들은 model의 depth를 증가시켜 inference latency를 유발하거나 baseline보다 성능이 크게 낮아지는 문제가 있었음
> - Inference latency: 모델이 입력에서부터 출력까지를 처리하는 시간
- 본 논문은 pretrained LLM의 parameters를 모두 freeze하고, rank decomposition matrice를 optimize함으로써 task-specific한 네트워크의 일부 parameters를 훈련시킨 방법을 제안함 (Low-Rank Adaptation, LoRA)

## 접근법
- Pre-trained LLM을 adaptation할 때, 실제 LLM의 parameters $W_0$는 freeze한 채, low-rank decomposition matrices $A$, $B$만 업데이트 되도록 함
> - $W_0 + \Delta{W}=W_0 + BA$
> - $W_0$는 gradient updates되지 않고, $B$와 $A$만 trainable함
- 이후 학습된 low-rank decomposition matrices로 task-specific한 inference 가능
> - $h=W_0{x}+\Delta{W}=W_0{x}+BAx$
- Init 상태일 때 $BA$는 zero-matrix임

## 실험결과
- GPT-3 175B에 적용했을 때 LoRA는 storage-/compute-efficient했음

## 의견
- /