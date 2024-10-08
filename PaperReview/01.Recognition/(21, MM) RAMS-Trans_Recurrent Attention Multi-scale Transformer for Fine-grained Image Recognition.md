# RAMS-Trans: Recurrent Attention Multi-scale Transformer for Fine-grained Image Recognition (MM, 2021)

[논문링크](https://dl.acm.org/doi/abs/10.1145/3474085.3475561)

<p align="center">
    <img width="600" alt='fig1' src="../img/hu2021rams.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT를 fine-grained visual recognition에 바로 적용하면 다음과 같은 문제가 발생함
> - ViT는 모두 같은 scale의 패치를 처리하므로, 이미지의 resolution이 크거나 이미지의 복잡도가 증가하는 경우 모든 정보를 효과적으로 담을 수 없으며, 반대로 이미지의 resolution이 작은 경우 local한 정보를 효과적으로 담을 수 없음
> - ViT는 네트워크의 전 과정에서 patch의 개수가 변하지 않으므로 receptive field가 항상 고정됨
- 따라서, 본 논문에서는 attention weight를 기반으로 multi-scale의 discriminative region을 학습하는 recurrent-attention multi-scale transformer (RAMS-Trans)를 제안함

## 접근법
- 먼저, input image는 하나의 Transformer 네트워크를 통과함
- 첫번째 Transformer에서 각 layer의 attention weights를 추출하고, attention roll-out을 기반으로 discriminative한 패치를 선택
- 선택된 patches를 기반으로 maximum connected region search algorithm을 적용하여 하나의 region을 추출함
- 추출한 region에 bilinear interpolation을 적용하여 마치 discriminative한 region을 zoom-in한 것과 같은 두 번째 input image를 생성 (multi-scale, receptive field 변화)

## 실험결과
- CUB, DOG, iNAT2017에서 SOTA 성능 달성

## 의견
- /