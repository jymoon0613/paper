# Visual Instruction Tuning (NIPS, 2023)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2023/hash/6dcf277ea32ce3288914faf369fe6de0-Abstract-Conference.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/liu2024visual.png?raw=true"></br>
    <em><font size=2>LLaVA network architecture.</font></em>
</p>

## 연구목적
- 최근 많은 open-source LLM이 제안되었고, 이들은 instruction tuning을 통해 상업용 LLM과 견줄만한 성능을 보이고 있지만 text-only라는 한계가 있음
> [!NOTE]
> In-context Learning vs. Instruction Learning
> - LLM은 pre-training 과정을 통해 방대한 양의 지식을 축적하고 있지만 이를 사용자의 의도에 맞게 응답으로 생성하는 데 어려움이 있음
> - **In-context learning**: 모델을 업데이트 하지 않고 효과적인 프롬프트 엔지니어링을 통해 사용자가 원하는 출력을 생성하도록 하는 방법
>   - 모델에게 질문에 대한 예시를 보여 줌으로써 원하는 형식의 출력을 유도 (e.g., one-shot, few-shot)
>   - 다만, 모델 자체에 충분한 정보를 가지고 있지 않다면, 프롬프트를 아무리 정교하게 구성한다고 해도 한계에 직면
> - **Instruction learning**: 사용자의 구체적인 지시(instruction)와 이에 대한 모델의 적절한 응답(output) pairs로 구성된 데이터셋을 사용하여 모델을 fine-tuning하는 방법
>   - LLM의 alignment ability를 개선
>   - 데이터셋으로 학습하므로 zero-shot, 즉, (예시 없이) 질문만으로 답변 도출 가능
- 본 논문에서는 instruction tuning을 language-image multimodal space로 확장한 visual instruction tuning을 제안

## 접근법
- ChatGPT와 GPT-4를 사용하여 image-text pairs를 multimodal instruction-output pairs로 확장시킴 (총 158K개)
- ChatGPT와 GPT-4는 text-only이기 때문에 이미지의 visual features를 prompt 형식으로 바꿔서 입력함
  - 이미지의 visual features를 이미지에 대한 설명을 담은 captions와 이미지 내 객체의 bounding boxes 좌표로 표현하여 전달함
- Response type으로는 conversation(58K), detailed description(23K), complex reasoning(77K)을 구성함
  - Conversation: object type, object 수, object 위치와 같은 이미지에 대한 사용자의 질문과 그에 대한 답변
  - Detailed description: 이미지에 대한 풍부하고 포괄적인 설명
  - Complex reasoning: 이미지와 관련된 복잡한 추론 (e.g., What challenges do these people face?)
- 모델 아키텍처는 Vicuna를 LLM으로 사용하며 visual features 인코딩을 위해 CLIP visual encoder를 사용함
- 입력 이미지가 CLIP visual encoder에 입력되어 인코딩되고, linear projection layer를 거쳐 word embedding space로 매핑됨
- Visual embedding과 instruction에 대한 word embedding을 concat하여 LLM에 입력으로 제공
- 다른 파라미터는 고정한 채로 projection layer에 대한 fine-tuning을 우선 진행하고, 이후 end-to-end fine-tuning을 진행함

## 실험결과
- LLaVA는 visual reasoning task에서 BLIP-2와 OpenFlamingo와 달리 사용자의 instructions를 정확히 따랐으며 GPT-4보다 더 좋은 답변을 제공했음
- ScienceQA task에서 SOTA 성능 달성

## 의견
- /
