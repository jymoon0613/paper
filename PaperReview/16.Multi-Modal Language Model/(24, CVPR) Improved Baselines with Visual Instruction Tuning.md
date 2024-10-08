# Improved Baselines with Visual Instruction Tuning (CVPR, 2024)

[논문링크](https://openaccess.thecvf.com/content/CVPR2024/html/Liu_Improved_Baselines_with_Visual_Instruction_Tuning_CVPR_2024_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/liu2024improved.png?raw=true"></br>
    <em><font size=2>LLaVA-1.5 achieves SoTA on a broad range of 11 tasks (Top), with high training sample efficiency (Left) and simple modifications to LLaVA (Right): an MLP connector and including academic-task-oriented data with response formatting prompts.</font></em>
</p>

## 연구목적
- LLM에 대한 많은 연구가 이루어졌지만 LLM을 훈련시키는 최고의 recipe이 무엇인가는 여전히 불명확함
- 본 논문에서는 LLM의 design choices를 탐구하기 위해 controlled setting에서 systematic study를 수행함
- 구체적으로, LLaVA를 베이스라인으로 input, model, data 측면에서의 contributions를 탐구함 (LLaVA-1.5)

## 접근법
### (1) Response Format Prompting
- LLaVA는 한 단어로 답변해야 하는 short-form answers를 잘 하지 못하며, yes/no 답변이 필요한 질문에서 항상 yes로 답하는 문제가 존재함
  - 이러한 형식의 데이터가 학습에 거의 포함되지 않았기 때문
- 반면 InstructionBLIP은 short-form answers에 overfit된 나머지 detailed reponses를 요구하더라도 항상 짧은 답변을 내놓는 경향이 있음
- LLaVA의 short-form answers를 향상시키면서 InstructionBLIP의 overfit 이슈를 방지하기 위해 VQA 시 prompt 형식을 더 구체적으로 변경함
  - 질문 이후 원하는 답변 형식을 명시 (e.g., Answer the question using a single word or phrase.)
### (2) Scaling the Data and Model
- LLaVA의 linear projection layer를 two-layer MLP로 교체함
- 학습 시 academic-task-oriented VQA datasets을 추가함
- 입력 이미지 스케일을 $336\times{336}$으로 변경함
- LLM을 13B로 scale-up함
### (3) Scaling to Higher Resolutions
- CLIP visual encoder의 입력 resolution이 $336\times{336}$이기 때문에 resolution을 더 올리는 것이 어려움
- 이를 해결하기 위해 higher resolution image를 visual encoder가 처리할 수 있는 크기의 여러 image pathces로 나누고, 각각을 따로 인코딩한 뒤 aggregate하는 방법을 사용함 (LLaVA-1.5-HD)

## 실험결과
- LLaVA-1.5는 15개의 벤치마크 중 11개에서 SOTA 성능 달성

## 의견
- /