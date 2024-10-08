# High-Quality Image Restoration Following Human Instructions (arXiv, 2024)

[논문링크](https://arxiv.org/abs/2401.16468)

<p align="center">
    <img width="700" alt='fig1' src="../img/conde2024high.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 사람의 text guidance(prompt)에 따라 image restoration을 수행하는 기법에 대해 다룸
> - 사람의 text instruction은 주어진 이미지의 어떤 degradations를 해결해야 하는지에 관한 명확한 guidance를 제공할 수 있음
- 본 논문에서 제안하는 InstrucIR은 all-on-one 모델로서 하나의 degradation 타입에만 국한된 것이 아니라 다양한 degradation 타입에 일반화될 수 있음
> - Image denoising, deraining, deblurring, dehazing 등

## 접근법
- 도메인 전문가뿐만 아니라 일반 성인, 아이들이 작성한 text prompts, 즉, in-the-wild human instructions을 활용하도록 하는 것이 중요함
- 이를 위해, GPT-4를 사용하여 서로 다른 degradation 타입에 대한 다양한 어투의 requests를 생성함
> - 총 10,000개의 prompts를 생성
- Text prompts는 pre-trainind text encoder(i.e., BGE-micro-v2)를 통해 embedding vectors로 매핑됨
- 이미지 모델로는 U-Net 구조의 NAFNet를 사용함
- Text embedding은 linear layer를 거쳐 mask $m_c$로 매핑되고, image feature $\mathcal{F}_c$와 channel-wise multiplication을 수행하여 $\mathcal{F}^\prime_c$를 얻음
- 모델은 ground-truth clean image와 restored image 간의 L1 loss를 optimize하는 방식으로 학습함

## 실험결과
- InstructIR은 image denoising, deraining, deblurring, dehazing, low-light image enhancement tasks에서 모두 SOTA 성능 달성
- 하나의 noisy image에 대해 다른 text prompts를 주면 해당 instruction에 맞게 restoration이 수행됨

## 의견
- /