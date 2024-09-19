# PLLaVA: Parameter-free LLaVA Extension from Images to Videos for Video Dense Captioning (arXiv, 2024)

[논문링크](https://arxiv.org/abs/2404.16994)

<p align="center">
    <img width="600" alt='fig1' src="../img/xu2024pllava.png?raw=true"></br>
    <em><font size=2>The framework of PLLaVA.</font></em>
</p>

## 연구목적
- 최근 image-text pairs로 학습된 MLLM이 좋은 image comprehension 능력을 보여줌에 따라 video comprehension을 향상시키기 위해 LLM을 video-text pairs로 fine-tuning하려는 시도가 있었음
- 하지만 이는 막대한 computing resources를 요구하며 video data annotations도 어렵기 때문에 기존의 pre-trained된 image-domain MLLM을 video data로 adapt하는 것이 더 실용적임
- 본 논문에서는 LLaVA-Next 모델을 베이스라인으로 image MLLM을 video tasks에 adapt하는 과정에서의 failure modes를 탐구하고, 이에 대한 해결방안이 적용된 Pool-LLaVA(PLLaVA)를 제안함

## 접근법
- Image MLLM을 video domain에 adapt하는 가장 간단한 방법은 각 video frame을 image encoder(e.g., CLIP-ViT)로 인코딩한 뒤, frame features를 concatenate하여 MLLM으로 입력하는 방법임 (n-frame method로 표기)
- 하지만 이러한 방법은 두 가지 문제를 야기함
  - (1) 모델 성능이 입력 prompt pattern에 매우 민감해짐
    - 모델이 훈련 시 사용된 prompt pattern으로 instruction을 받으면 정상적으로 출력이 생성되는 반면, 같은 내용을 prompt pattern만 바꾸어 입력하는 경우 출력이 매우 짧아지거나 아예 출력이 되지 않는 경우가 발생
    - 분석 결과, 몇몇 frame feature tokens가 다른 tokens에 비해 훨씬 큰 norm을 가지며 degradation을 유발함
  - (2) LLM을 scale-up해도 성능 향상이 거의 없음 (scaling 효과가 없음)
    - 7B, 13B, 34B 모델 간 성능 차이가 거의 없음
    - 분석 결과, image-text 데이터셋과 비교하여 video-text 데이터셋의 낮은 퀄리티가 degradation을 유발함
- 먼저, (1)에 대한 해결책으로 frame information을 더 효과적으로 처리하기 위해 pooling을 사용함
  - CLIP-ViT로 인코딩된 frame features에 parameter-free한 adaptive average structure pooling을 적용
- (2)에 대한 해결책으로 video MLLM에 post-training optimization을 적용함
  - 훈련된 video MLLM의 parameters와 기존 MLLM parameters를 적절히 blending함

## 실험결과
- Pooling을 적용했을 때 특정 frame features의 norm이 커지는 현상이 줄어듦
- PLLaVA는 videoQA 및 video captioning tasks에서 SOTA 성능을 달성함

## 의견
- /