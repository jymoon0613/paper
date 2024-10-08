# TransUNet: Transformers Make Strong Encoders for Medical Image Segmentation (arXiv, 2021)

[논문링크](https://arxiv.org/abs/2102.04306)

<p align="center">
    <img width="700" alt='fig1' src="../img/chen2021transunet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 처음으로 Transformer 구조를 medical image segmentation에 적용해봄 (TransUNet)
- TransUNet은 CNN과 Transformer를 적절히 결합한 hybrid 구조임
> - CNN으로부터 high-resolution의 detailed spatial information이 담긴 features를 활용함
> - Transformer로부터 global context를 encoding함

## 접근법
- Transformer를 medical image segmentation에 적용하는 가장 쉬운 방법은 Transformer encoder blocks를 거친 image tokens를 upsampling하여 바로 예측에 사용하는 것 (ViT-None)
> - Output image tokens의 resolution이 original resolution이랑 차이가 크기 때문에 low-level details의 손실이 크고, 이는 suboptimal한 성능으로 이어짐
- TransUNet은 이러한 문제를 보완하기 위해 CNN-Transformer의 hybrid 구조를 사용
- 먼저 CNN으로부터 feature map을 추출하고, 추출된 feature map에 patch embedding을 적용하여 Transformer로 입력함
> - 원본 이미지를 바로 embedding하여 Transformer로 입력하는 기존 방식과 다름
- Final segmentation mask를 생성하기 위해 multiple upsampling steps로 구성된 cascaded upsampler(CUP)를 제안함
> - Transformer blocks를 거친 hidden features를 2D로 reshape함
> - Reshape된 hidden features를 원본 이미지 resolution으로 순차적으로 upsampling하되 각 upsampling stage에서 CNN의 중간 feature map을 결합해나감

## 실험결과
- TransNet은 ViT-None에 비해 성능이 크게 개선됨
- CUP는 단순 upsampling보다 성능 향상에 효과적이었음

## 의견
- /