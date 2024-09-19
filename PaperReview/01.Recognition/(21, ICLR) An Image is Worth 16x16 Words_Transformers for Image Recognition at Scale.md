# An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale (ICLR, 2021)

[논문 링크](https://arxiv.org/abs/2010.11929)

<p align="center">
    <img width="600" alt='fig1' src="../img/dosovitskiy2020image.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transformer는 NLP에서 표준 모델로서 널리 사용되고 있음
- Transformer의 뛰어난 효율성과 확장성 덕분에 모델 크기 혹은 데이터셋의 크기를 매우 키워도 성능 포화의 조짐이 보이지 않음
- CV에서는 여전히 CNN이 주로 사용되고 있음
> - 변화의 시도는 있었지만 아직 성능 검증이 되지는 않음
- 본 논문에서는 standard Transformer 구조를 image에도 적용해보자 함 (Vision Transformer, ViT)
- 이를 위해 각 이미지는 patch 단위로 분할되어야 하며, 이때 각 image patch는 NLP의 word token과 같은 역할을 함
- ImageNet과 같은 중간 크기의 데이터셋으로 학습을 시켰을 때, ViT는 ResNet 기반의 모델보다 약간 뒤쳐진 성능을 보였음
> - 이는 Transfomer가 CNN과 달리 inductive bias가 부족하기 때문임
- 하지만, ViT를 매우 큰 데이터셋에서 사전 훈련시키고, 보다 적은 데이터가 있는 downstream task로 transfer시켰을 때는 성능이 매우 좋았음
> - Large scale training이 inductive bias를 능가함

## 접근법
- 기존 Transformer를 가능한 그대로 사용하고자 했음
> - NLP Transformer 효율성과 확장성을 그대로 이식하기 위해

### (1) Vision Transformer (ViT) 
- 입력 이미지를 고정된 크기의 이미지 토큰(패치)으로 나누고 각 패치를 $D$ dimensions로 linear embedding함
> - Convolution으로 대체 가능함 (hybrid)
- 이때 BERT의 CLS token과 마찬가지로 image representation을 저장하는 learnable embedding token을 정의함 (patch sequence에 추가됨)
- 각 패치의 위치 정보를 보존하기 위해 patch embedding에 positional embedding을 더해줌
- 처리가 완료된 patch sequence가 ViT encoder로 입력됨
> - ViT(Transformer) encoder: multi-head self-attention, MLP blocks, layer normalization, residual connection
- ViT encoder는 CNN보다 image-specific한 inductive bias가 적음
> - CNN은 각 레이어에서 locality, translation equivalance 등의 inductive bias를 포함
> - ViT는 MLP만이 local 및 translation equivalance의 inductive bias를 포함하며, self-attention은 global함

### (2) Fine-tuning and Higher Resolution 
- ViT를 large-scale 데이터셋에서 사전 학습시킨 후, downstream tasks로 fine-tuning함
> - Task에 적합한 classification head만 새로 정의
- Fine-tuning 시, 사전 학습보다 큰 image resolution을 사용하는 것이 더 효과적이었음
> - 더 큰 resolution 이미지 사용 시, patch size는 고정함 (sequence length가 증가)
> - ViT는 임의의 seqeunce length를 처리할 수 있지만 positional embedding은 수정해주어야 함
> - 이를 위해 pre-trained된 positional embedding은 기존 이미지에서의 위치에 따라 interpolation해줌

## 실험결과
- ResNet과 ViT의 성능을 주로 비교함
> - 다양한 크기의 데이터셋과 여러 벤치마크 tasks에서 평가함
- JFT-300M 데이터셋으로 사전 훈련된 ViT는 모든 벤치마크에서 더 적은 연산량으로도 ResNet 기반 모델의 성능을 능가함
- 데이터셋 크기의 중요성을 확인하기 위해 사전 훈련 데이터셋의 크기를 조절하면서 ViT를 훈련시킴
- ImageNet-1K 및 ImageNet-21K와 같은 작은 데이터셋에서 사전 훈련 시 ViT-L은 ViT-B보다 낮은 성능을 기록함
> - 모델 크기 증가의 효과가 없었으며, JFT-300M을 사용할 때만 모델 크기 증가 효과가 나타남
> - 또한, ImageNet으로 사전 훈련 시 ResNet 기반 모델보다 성능이 안좋았고 JFT-300M을 사용할 때만 ViT의 성능이 더 좋았음
- 또한, JFT-300M 데이터셋에서 9M, 30M, 90M 개의 데이터를 랜덤하게 추출하여 사전 훈련했을 때와 전체 다 사용했을 때의 성능을 평가함
> - 90M 이상의 데이터를 사용했을 때 ViT의 성능이 ResNet 기반 모델을 능가함
- 이러한 결과는 CNN의 inductive bias가 데이터셋이 매우 적을 때 유용하며, 데이터셋이 커지면 (ViT처럼)데이터로부터 직접 관련 패턴을 학습하는 것만으로도 충분하고 오히려 더 좋다는 것을 증명함
- 동일한 연산량에 대해 ViT의 성능이 ResNet 기반 모델보다 좋았음
- 각 attention head의 attention distance를 조사해본 결과 일부 attention head는 네트워크 초기부터 매우 global한 interaction을 모델링하고 있었으며, 하위 layer에서 매우 local한 interaction만을 모델링하는 head도 있었음
- ViT encoder의 self-attention은 image에서 classification과 연관 있는 regions에 attend하고 있음
- Masked patch prediction을 사용하여 ViT를 self-supervised manner로 사전 훈련시켰을 때 scratch model보다 성능이 증가하였음
> - 하지만, supervised manner로 사전 훈련시켰을 때보다는 성능이 낮았음

## 의견
- Transformer는 CNN 고유의 Inductive Biases가 부족하기 때문에 불충분한 양의 데이터에 대해 훈련되었을 때 일반화 성능이 낮음 (ResNet 보다 몇 퍼센트 포인트 낮은 정확도) 