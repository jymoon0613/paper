# EfficientViT-SAM: Accelerated Segment Anything Model Without Performance Loss (arXiv, 2024)

[논문링크](https://arxiv.org/abs/2402.05008)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2024efficientvit.png?raw=true"></br>
    <em><font size=2>Macro Architecture of EfficientViT-SAM-XL.</font></em>
</p>

## 연구목적
- Segment Anything Model(SAM)은 매우 뛰어난 zero-shot image segmentation 성능을 보여주고 있음
- 하지만 SAM은 연산복잡도가 매우 커 time-sensitive한 상황에서 사용하기 어려움
- SAM의 연산복잡도를 낮추기 위한 많은 시도가 있었지만, 이들은 대부분 큰 성능 하락을 수반한다는 문제가 있었음
- 본 논문에서는 이러한 한계를 해결하기 위해 EfficientViT-SAM을 제안함
> - SAM의 이미지 인코더를 가벼운 EfficientViT로 대체함
> - 또한, 효과적인 학습을 위한 훈련 방식을 재설계함

## 접근법
- SAM의 이미지 인코더로 EfficientViT를 사용함
> - 네트워크의 초기 단계에서는 convolution blocks를 사용하고 마지막 두 단계에서 EfficientViT 모듈을 사용
> - 마지막 세 단계의 feature maps를 fusion하고 mobile convolution blocks(MBConv blocks)로 구성된 neck을 거쳐 SAM head로 전달함
- EfficientViT-SAM 훈련을 위해 우선 knowledge distillation을 통해  SAM-ViT-H의 이미지 임베딩을 EfficientViT로 전이시킴
- Prompt encoder 및 mask decoder는 SAM-ViT-H의 가중치로 초기화시켜준 뒤 EfficientViT-SAM 전체를 end-to-end로 학습시킴

## 실험결과
- Throughput을 기준으로 runtime efficiency를 평가한 결과 EfficientViT-SAM은 SAM보다 최소 17배에서 최대 69배 개선된 속도를 기록함
- Zero-shot segmentation 성능을 평가한 결과 EfficientViT-SAM은 SAM보다 좋은 성능을 기록함
- 정성적으로 평가한 결과 EfficientViT-SAM은 큰 object뿐만 아니라 작은 object에 대해서도 매우 잘 segment함

## 의견
- /