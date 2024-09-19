# Guiding Instruction-based Image Editing via Multimodal Large Language Models (ICLR, 2024)

[논문링크](https://arxiv.org/abs/2309.17102)

<p align="center">
    <img width="700" alt='fig1' src="../img/fu2023guiding.png?raw=true"></br>
    <em><font size=2>Overview of MLLM-Guided Image Editing (MGIE).</font></em>
</p>

## 연구목적
- 본 논문에서는 instruction-based image editing 모델인 MLLM-Guided Image Editing(MGIE)을 제안함
> - MGIE는 multimodal large language model(MLLM)와 diffusion model로 구성됨
> - MLLM은 instructions을 해석하고 visual-related guidance를 제공하며, diffusion model은 이에 기반한 image editing을 수행함
- MGIE는 global한 photo optimization부터 local object alteration 까지 Photoshop-style 수정이 가능함

## 접근법
- MLLM은 강력한 자연어 생성 능력을 지닌 LLM을 확장하여 입력 이미지로부터 논리적인 responses를 생성하도록 훈련된 모델임
> - MLLM은 pre-trained LLM과 함께, visual features를 추출하는 visual encoder(CLIP-L), visual features를 language modality로 변환하는 adapter로 구성됨
- MGIE는 입력 이미지 $\mathcal{V}$를 instruction $\mathcal{X}$에 따라 goal image $\mathcal{O}$로 변환함
- MGIE는 부정확한 instructions를 처리하기 위해 MLLM을 사용하며, MLLM은 $\mathcal{X}$로부터 더 간결하면서 명시적인 instructions $\mathcal{E}$를 추출하도록 훈련됨
- 이후 editing head는 sequence-to-sequence 방식으로 instructions를 meaningful latent tokens $\mathcal{U}$로 임베딩함
- Latent diffusion model $\mathcal{F}$는 $\mathcal{U}$에 따라 $\mathcal{V}$를 $\mathcal{O}$로 변환함

## 실험결과
- MLLM을 통해 goal image에 대한 정확한 alignment를 제공하는 것이 editing quality에 긍정적인 영향을 주었음
- Human evaluation 결과, MGIE는 기존의 LGIE, InsPix2Pix에 비해 높은 사용자 선호도를 기록하였음 

## 의견
- /