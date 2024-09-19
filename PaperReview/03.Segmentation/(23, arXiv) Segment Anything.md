# Segment Anything (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2304.02643)

<p align="center">
    <img width="600" alt='fig1' src="../img/kirillov2023segment.png?raw=true"></br>
    <em><font size=2>Segment Anything Model (SAM) overview.</font></em>
</p>

## 연구목적
- NLP 분야에서, web-scale 데이터셋에서 pre-trained된 foundation model이 강력한 zero-/few-shot generalization 성능을 기록하고 있음
> - 이러한 강력한 generalization 능력은 language model이 hand-crafted text 입력에 대해 적절한 textual response를 생성하도록 하는 'prompt engineering'에 의해 구현됨
> - 모델이 충분히 scaling되고 풍부한 데이터로 학습될 수 있다면 task-specific하게 fine-tuning된 모델과 유사한, 혹은 더 뛰어난 성능을 보임
- Computer vision 분야에서는 CLIP과 같이 image와 text pairs를 동시에 사용하는 방식으로 foundation model이 개발되고 있지만 NLP와 비교했을 때는 부족한 수준임
- 본 논문에서는 image segmentation을 위한 foundation model을 제안함 (Segment Anything Model, SAM)
> - Promptable한 segmentation model을 개발하고, 강력한 generalization이 가능한 task를 사용하여 다양한 데이터셋에서 pre-train시킴
> - Model, data, task 측면에 집중함

## 접근법
- NLP에서 foundation model 훈련에 주로 사용되는 next token prediction task와 같이 segmentation foundation model을 위한 promptable segmentation task를 정의함
- Promptable segmentation task는 임의의 prompt가 주어졌을 때 valid한 segmentation mask를 출력하도록 하는 task임
> - Prompt는 이미지 속 segment할 object의 정보를 담고 있음
> - Prompt가 text일 필요는 없으며 foreground/background points, bbox 정보가 될 수 있음
> - Prompt가 모호하거나 multiple objects를 지칭하더라도 valid한 segmentation mask를 생성해야 함
- 다음으로, SAM 모델 구조는 image encoder, prompt encoder, mask decoder로 구성됨
- Image encoder는 MAE로 pre-trained된 ViT임
- Prompt encoder는 입력 prompts의 타입에 따라 CLIP을 사용하거나 convolutional block을 사용하는 등 다르게 구성함
> - Sparse (points, boxes, text) prompts vs. dense (masks) prompts
- Mask decoder는 image embedding, prompt embedding, output token을 mask로 매핑하는 역할을 함
> - Mask prediction head가 부착된 Transformer decoder block을 사용함
- 모호한 prompt에 대비하기 위해 SAM은 하나의 prompt에 대해 다수(i.e., 3개)의 valid masks를 생성함
> - 학습 시에는 3개의 valid masks 중 loss가 가장 작은 하나의 mask에 대해서만 back prop. 진행
- 대규모 학습을 위한 segmentation masks가 충분하지 않기 때문에 직접 data engine을 설계하여 1.1B 크기의 mask 데이터셋인 SA-1B를 제작함
> - Data engine은 SAM의 학습 단계에 따라 mask labeling 단계의 참여 수준을 조절하는 것을 의미함
> - 초기에 public 데이터셋으로 학습된 SAM은 professional annotators의 작업을 보정하는 기초적인 도구로 사용됨
> - 추가된 data로 SAM이 훈련될수록 SAM이 자동으로 unlabeled image의 segmentation mask를 직접만듦

## 실험결과
- Zero-shot segmentation 성능을 평가한 결과, SAM은 해당 데이터셋에서 fine-tuning된 기존의 segmentation model들과 비슷하거나 더 좋은 성능을 기록함으로써 segmentation foundation model로 충분함을 증명
- 이러한 강력한 generalization 성능은 zero-shot edge detection, zero-shot object proposals, zero-shot text-to-mask tasks에서도 마찬가지였음

## 의견
- /