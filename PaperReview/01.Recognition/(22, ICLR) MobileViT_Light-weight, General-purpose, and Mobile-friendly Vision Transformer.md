# MobileViT: Light-weight, General-purpose, and Mobile-friendly Vision Transformer (ICLR, 2022)

[논문링크](https://arxiv.org/abs/2110.02178)

<p align="center">
    <img width="700" alt='fig1' src="../img/mehta2021mobilevit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Light-weight CNN의 발전과 비교했을 때 light-weight ViT의 발전 속도는 더딘편
> - CNN은 최적화과 비교적 쉽고 task-specific한 networks로의 통합이 간단한
> - 반면 ViT는 성능 향상을 위해 많은 weights가 필요하고, 최적화가 상대적으로 어려우며, 많은 data augmentation 및 regularization 기법이 필요하다는 점에서 경량화가 쉽지 않음
> - ViT는 CNN과 달리 image-specific한 inductive bias가 부족하기 때문
- 본 논문에서는 ViT에 CNN의 특성(spatial inductive biases, less sensitivity to data augmentation)을 결합하여, 가벼운(light-weight), general-purpose, low latency의 ViT를 제안함 (MobileViT)

## 접근법
- MobileViT의 핵심 아이디어는 Transformers를 convolutions로 사용하여 global representations를 학습하는 것
> - 이는 네트워크에 convolution-like properties를 암묵적으로 주입할 수 있고, 간단한 training recipes로도 학습가능하게 하며, 여러 downstream 아키텍처(e.g., DeepLabV3)로 쉽게 통합될 수 있도록 함
- Input tensor에 대해 MobileViT block은 nxn convolution과 1x1 convolution을 적용함
> - nxn convolution은 local spatial information을 인코딩하고, 1x1 convolution은 입력 tensor를 더 높은 차원의 (channel) dimension으로 매핑함
- 이후 tensor를 일정한 크기의 patch regions로 분할하고, 분할된 patches에 대해 self-attention 수행
- 이후 입력 tensor에 self-attention이 완료된 tensor를 concat함
- Concat된 tensor에 또다른 nxn convolution 적용
- Convolution을 통해 local inductive bias를 적용하면서 self-attention으로 global processing을 수행
- 또한, 학습시 input resolution을 랜덤하게 샘플링하여 multi-scale training 수행
> - 이때 resolution에 따라 batch size를 조절하여 GPU utilization을 최대로 활용

## 실험결과
- ImageNet-1K 데이터셋에서 DeiT보다 2배 적은 FLOPs로 1.8% 더 높은 정확도를 기록
- Light-weight CNNs와 비교했을 때 비슷한 파라미터로 더 좋은 성능 기록
- Heavy-weight CNNs와 비교했을 때도 오히려 성능이 더 좋았음
- Light-weight ViT와 비교했을 때도 동일 파라미터 대비 성능이 더 좋았으며, heavy ViT보다도 더 좋거나 거의 비슷했음
- Detection task의 backbone으로 사용했을 때 heavy CNN을 backbone으로 사용할 때보다 오히려 성능이 좋았음

## 의견
- /