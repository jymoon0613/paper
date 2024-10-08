# Pyramid Vision Transformer: A Versatile Backbone for Dense Prediction Without Convolutions (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Wang_Pyramid_Vision_Transformer_A_Versatile_Backbone_for_Dense_Prediction_Without_ICCV_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2021pyramid.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 image classification task에서 좋은 성능을 기록하였지만 dense prediction이 필요한 object detection 및 segmentation에 직접 적용시키기에는 어려움
> - ViT는 단일 scale의 feature map을 처리하며 input resolution도 작음
> - Input resolution을 키우게 되면 계산 비용 및 메모리 소모량이 매우 커짐
- 따라서, 본 논문에서는 CNN의 대안으로서 image-level prediction 및 dense prediction을 수행할 수 있는 pure Transformer 구조인 Pyramid Vision Transformer(PVT)를 제안함
- PVT는 (1) dense prediction을 위한 high-resolution representation을 학습하기 위해 fine-grained한 image patches를 사용하며, (2) 계산 비용을 줄이기 위해 점진적으로 input의 sequence length를 줄이고, (3) spatial-reduction attention layer를 사용하여 high-resolution features를 학습할 때의 리소스 소모를 추가적으로 줄임

## 접근법
- 입력 이미지를 4x4의 fine-grained한 patch로 나누고 patch embedding을 진행
- 각 stage가 진행되면서 feature map의 크기(or sequence length)가 줄어듦: progressive shrinking strategy 사용
- 한 stage를 거친 후 다시 2D로 reshape된 feature map에 대해, 다른 크기의 patch size를 적용하여 sequence length를 조절
- 즉, patch embedding을 다시 수행 (positional embedding도 마찬가지로 다시 수행)
- 각 Transformer block에서는 fine-grained patches로 이루어진 high-resolution의 feature map을 처리해야 하므로 이전의 multi-head attention(MHA) layer 대신 spatial-reduction attention(SRA) layer를 사용
- SRA는 똑같이 query, key, value vectors를 사용하지만 key와 value의 spatial scale을 attention 연산 전에 축소시킴: 연산량을 크게 감소시킴

## 실험결과
- PVT는 유사한 parameter 수의 CNN 모델보다 image classification에서 좋은 성능을 기록
- 또한, 기존 ViT 혹은 DeiT와 비교했을 때 비슷하거나 더 적은 연산량으로 경쟁력있는 성능 기록
- ViT와 DeiT를 classification에서 압도하지는 못했지만 PVT는 이 둘과 달리 dense prediction tasks에 적용가능하다는 장점이 있음
- Object detection 및 segmentation에서도 CNN 백본보다 좋은 성능을 보였으며, 이는 PVT가 CNN 백본을 대체할 수 있음을 증명함

## 의견
- /