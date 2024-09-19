# YOLO-World: Real-Time Open-Vocabulary Object Detection (arXiv, 2024)

[논문링크](https://arxiv.org/abs/2401.17270)

<p align="center">
    <img width="700" alt='fig1' src="../img/cheng2024yolo.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Vision-language modeling을 통해 YOLO를 open-vocabulary detection task로 확장 (YOLO-World)
- Pre-trained CLIP을 text encoder로 사용하며, text features와 image featrures 간의 visual-semantic representation을 강화하기 위해 Vision-Language Path Aggregation
Network(RepVL-PAN)을 설계
> - Inference 시 text encoder는 freeze되며, text embedding은 RepVL-PAN의 weights로 re-parameterize됨
- 또한, large-scale datasets에서의 region-text contrastive learning을 통해 YOLO를 open-vocabulary에 적합하게 pre-training함

## 접근법
- 기존의 object detectors가 bboxes $B_i$와 category labels $c_i$ 간의 대응관계를 학습했다면, open-vocabulary setting에서는 bboxes $B_i$와 이에 해당하는 text $t_i$ 간의 대응관계를 학습
> - $t_i$는 category name, object descriptions 등이 될 수 있음 
> - Region-text pairs 간의 contrastive loss를 계산하면서 학습
- YOLO-World는 YOLO detector(YOLOv8), text encoder(CLIP), RepVL-PAN으로 구성됨
> - YOLO detector는 입력 이미지로부터 multi-scale features를 추출하고, text encoder는 입력 text로부터 text embeddings를 추출
> - RepVL-PAN은 text/image representation을 fusion함
- RepVL-PAN은 text embeddings와 multi-scale image features를 top-down, bottom-up 형식으로 순차적으로 fusion함
- Inference 단계에서는 prompt-then-detect 전략을 사용함
> - 사용자는 captions나 categories를 포함하는 custom prompts를 입력함
> - Freeze된 text encoder는 prompts를 인코딩하고 offline vocabulary embeddings를 출력함
> - Online vocabulary를 사용할 때에 비해 훨씬 가벼운 detector를 활용할 수 있음

## 실험결과
- YOLO-World pre-training 이후 LVIS 데이터셋에서의 zero-shot detection 성능을 평가했을 때 SOTA 성능을 기록함
> - 또한, inference speed도 가장 빨랐음
- COCO 데이터셋에서의 closed-set detection 성능을 평가했을 때도 기존 YOLOv8 대비 향상된 성능을 보여줌

## 의견
- /