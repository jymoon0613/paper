# Swin-Unet: Unet-like Pure Transformer for Medical Image Segmentation (ECCV-W, 2022)

[논문링크](https://arxiv.org/abs/2105.05537)

<p align="center">
    <img width="600" alt='fig1' src="../img/cao2022swin.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Image segmentation은 medical image analysis에서 매우 중요함 (i.e. computer-aided diagnosis, image-guided clinical surgery)
- 현재까지 제안된 medical image segmentation methods는 주로 fully convolutional neural network(FCNN) 기반의 U-shaped 구조를 사용함
- 가장 대표적인 네트워크인 U-Net은 대칭적인 Encoder-Decoder 구조를 사용하며 skip-connection으로 각각의 layer를 연결함
> - Encoder에서는 일련의 conv layers와 down-sampling을 적용하여 큰 receptive fields에서 deep features를 추출함
> - Decoder에서는 추출된 deep features를 input resolution과 맞추어 pixel-level semantic prediction을 학; 위해 연속적으로 up-sample함
> - 이때 각 encoder layer의 서로 다른 scale의 high-resolution features는 skip-connection을 통해 decoder의 각 layer의 features와 결합됨
> - 이는 encoder의 down-sampling 과정에서 생기는 정보의 손실을 보완하기 위한 것임
- 이러한 U-Net을 기반으로 성능이 뛰어난 여러 모델들(CNN 기반)이 제안되었지만, 여전히 medical applications에 상용화될 수 있을 만큼의 성능 요건에는 도달하지 못함
- 특히, CNN 기반의 모델들은 conv 연산의 locality 특성 때문에 global 혹은 long-range semantic information interaction을 모델링하지 못함
> - 이를 해결하기 위해 최근에는 dilated conv, self-attention, image pyramids와 같은 방법이 도입되기도 했지만 여전히 부족함
- 최근에 널리 사용되는 ViT는 이러한 long-range dependency를 모델링하는 데 최적화되어 있음
- 또한, Swin Transformer와 같은 hierarchical한 ViT는 recognition tasks를 넘어 segmentation, detection과 같은 dense prediction task에서도 ViT가 널리 사용될 수 있도록 함
- 따라서, 본 논문에서는 Swin Transformer의 뛰어난 성능을 2D medical image segmentation에 사용하는 Swin-Unet 아키텍처를 제안함
> - Swin-Unet은 encoder, decoder, bottlneck이 모두 Swin Transformer block으로 구성되어 있음

## 접근법
- 먼저 2D medical image는 non-overlapping patches로 embedding되어 deep features를 output하기 위해 encoder를 통과함
- 이후 patch expanding layer를 사용하는 Swin Transformer block으로 구성된 decoder를 통과함
- 마지막 decoder layer를 거친 뒤 추가로 linear layer를 거쳐 최종 예측을 수행함
- Encoder는 일반 Swin Transformer block과 동일함(patch merging도 사용)
> - Patch merging 사용
> - 3-stage로 구성 -> 총 6개의 Swin Transformer blocks로 구성됨
- Encoder를 거친 뒤 bottleneck을 거쳐 추가로 deep features를 refine해줌
> - 이때의 bottleneck은 feature dimension과 resolution이 유지되도록 정의된 Swin Transformer block 2개로 구성함
- Decoder도 3-stage, 총 6개의 Swin Transformer blocks로 구성됨
> - 단, patch merging 대신 patch expanding을 사용함
> - Patch expanding layer는 feature map의 resolution을 2배로 늘리고, dimension을 1/2로 줄임
> - Patch expanding layer는 입력 feature map에 대해 linear layer로 dimension을 2배로 늘리고, reshape을 적용하여 resolution expand/dimension reduction을 수행
- Decoder의 각 stage에서 대응되는 encoder stage의 features를 skip-connection을 통해 전달받음

## 실험결과
- Synapse, ACDC datasets에서 모두 SOTA 달성

## 의견
- /