# Neighborhood Attention Transformer (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Hassani_Neighborhood_Attention_Transformer_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/hassani2023neighborhood.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT의 self-attention은 input resolution에 quadratic한 복잡도를 지니며, 이는 ViT가 detection이나 segmentation 같이 주로 high input resolution을 요구하는 tasks에 쉽게 적용되지 못하도록 함
- 또한 CNN은 locality와 같은 inductive biases를 통해 성능적 이점을 취할 수 있는 반면, inductive bias가 없는 ViT는 상당한 양의 데이터 혹은 복잡한 training procedure을 통해 이러한 inductive biases를 학습해야 함
- 따라서, 본 논문에서는 위의 문제를 해결하기 위해 sliding window attention mechanisms 중 하나인 Neighborhood Attention(NA)을 제안함
> - 각 pixel의 attention span을 해당 pixel의 nearest neighborhood로 제안함
> - Span이 증가할수록 vanilla self-attention과 유사해짐
> - Translation equivariance 특성을 유지함

## 접근법
- SwinT의 shifted window self-attention(SWSA)는 가장 대표적인 local attention 방법임
- SwinT는 입력을 여러 partitions로 나누고 각 partition 별로 self-attention을 수행하며, out-of-window interactions를 위해 partition lines를 이동시켜주어야 함
- 하지만, self-attention을 local하게 수행하는 가장 direct한 방법은 각 pixel이 그들의 neighboring pixels에 attend하도록 하는 것임
- 이 경우, pixels는 그들 주위의 dynamically shifted window를 지니게 되는 것과 마찬가지이며, manual partition shift와 같은 과정이 더 이상 필요하지 않게됨
- 또한, 이러한 dynamic하게 제한되는 self-attention은 SwinT와 달리 translational equivariance를 유지할 수 있다는 장점도 있음
- 이러한 motivation을 바탕으로 Neighborhood Attention(NA)을 제안함
- NA에서 입력 token vector는 k개의 neighboring tokens에 대해서만 self-attention을 수행함
- 하지만, 모든 token 별로 neighboring tokens를 추출해야 하므로 연산이 매우 느려지고 메모리 소모량이 크게 증가함
- 따라서, 이를 해결하기 위해 효율적인 CPU 및 CUDA kernels를 개발하고 Python package로 배포함
- NA를 바탕으로 Neighborhood Attention Transformer(NAT) 아키텍처를 디자인함

## 실험결과
- Classification, detection, segmentation tasks에서 모두 좋은 성능을 기록함
> - SwinT 및 ConNeXt 모델을 능가함

## 의견
- / 