# Alpha-CLIP: A CLIP Model Focusing on Wherever You Want (CVPR, 2024)

[논문링크](https://openaccess.thecvf.com/content/CVPR2024/html/Sun_Alpha-CLIP_A_CLIP_Model_Focusing_on_Wherever_You_Want_CVPR_2024_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/sun2024alpha.png?raw=true"></br>
    <em><font size=2>Alpha-CLIP vs. other methods of region-focusing.</font></em>
</p>

## 연구목적
- CLIP은 이미지의 전반적인 visual features를 추출하지만 더 세분화된 인식을 위해 이미지의 특정 부분에 대한 fine-grained features를 추출하는 것도 중요함
- 이를 위해 cropping 혹은 masking에 기반한 region-focused CLIP methods가 제안되었지만 이러한 방식은 이미지의 contextual information을 훼손한다는 문제가 있었음
- 이미지 내 regions of interest를 원 혹은 mask contour로 강조하는 region-focused CLIP methods도 제안되었지만 마찬가지로 원본 이미지의 contents를 훼손한다는 문제가 있었음
- 본 논문에서는 원본 이미지를 훼손하지 않는 region-focused CLIP 기법을 제안함
- 이미지의 RGB channels와 함께, 이미지의 지정된 영역에 focus하면서 전반적인 contextual information은 유지하기 위한 alpha channel을 입력으로 같이 투입함 (Alpha-CLIP)

## 접근법
- 모델 학습을 위해 우선 대규모 RGBA(RGB channels + Alpha channel) 형식의 region-text pair 데이터가 필요함
- GRIT 데이터셋의 이미지를 사용하며 GLIP과 CLIP을 사용하여 bbox 형식의 region-text pairs를 추출하고, SAM을 사용하여 이를 object mask 형식의 region-text pairs로 변경함
- 또한, ImageNet 데이터셋에 SAM을 적용하여 몇개의 foreground object masks를 생성하고, object masks에 따라 foreground objects만 crop한 뒤 BLIP-2를 사용하여 captioning을 수행함
- 모델 구조는 기존 CLIP의 image encoder에 alpha channel 입력을 처리하는 Alpha Conv layer를 추가함
- 훈련 시 변경된 image encoder만 훈련되며 text encoder는 freeze됨

## 실험결과
- Zero-shot image classification task에서 기존 CLIP 및 여러 CLIP의 변형 methods보다 좋은 성능을 기록함
  - Alpha map으로 아무것도 제공하지 않았을 때, bbox 형식으로 제공했을 때, mask 형식으로 제공했을 때 순으로 성능이 높아짐
- 이외에도 Zero-shot referring expression comprehension(REC), Open vocabulary detection, region-level captioning에서도 좋은 성능을 기록

## 의견
- /