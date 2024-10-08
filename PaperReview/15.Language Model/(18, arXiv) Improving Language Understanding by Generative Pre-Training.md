# Improving Language Understanding by Generative Pre-Training (arXiv, 2018)

[논문링크](https://www.cs.ubc.ca/~amuham01/LING530/papers/radford2018improving.pdf)

<p align="center">
    <img width="600" alt='fig1' src="../img/radford2018improving.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Labeled data는 매우 부족하고, labeled data 수집은 시간 및 비용이 많이 필요하므로 unlabeled data를 최대한 이용하는 것이 중요함
- 또한, unsupervised한 pre-training은 supervised learning의 성능 향상에도 도움이 될 수 있음
- 하지만 NLP에서 unsupervised learning은 (1) 어떤 optimization objective가 transferrable한 representation을 학습하는 데 효과적인지 불분명하고, (2) 학습된 representation을 target task에 효과적으로 전달하는 검증된 방법이 없다는 점에서 challenging함
- 본 논문에서는 language understanding task에서 unsupervised한 pre-training과 supervised fine-tuning을 결합한 semi-supervised approach를 탐구하고자 함
- 연구의 목표는 여러 tasks에 쉽게 transfer될 수 있는 universal한 representation을 학습하는 것
- 모델 구조로는 Transformer 사용

## 접근법
- 학습은 두 단계로 구성: large unlabeled corpus에서의 pre-training, task-specific labeled data를 사용한 fine-tuning
- Transformer decoder 구조를 사용하여 language modeling으로 pre-training: 주어진 context window 내에서 한 position의 token을 모든 이전의 token을 사용하여 예측
- pre-trained된 모델에 layer를 추가하고 task에 맞게 fine-tuning함
- 또한, fine-tuning 시 pre-train의 objective function을 loss의 일부로 같이 사용함
- 이러한 방식은 supervised model의 generalization을 향상시키고 수렴을 가속화함
- 또한, 제안하는 구조 및 방법은 pre-train한 모델의 구조를 크게 바꾸지 않더라도 다양한 task에 fine-tuning이 가능함: task에 적합하게 입력 데이터의 구조만 변경시켜주면 됨

## 실험결과
- 여러 NLP tasks에서 SOTA 성능 달성

## 의견
- /