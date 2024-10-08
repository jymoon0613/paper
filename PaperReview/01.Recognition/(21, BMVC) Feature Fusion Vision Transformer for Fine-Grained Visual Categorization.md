# Feature Fusion Vision Transformer for Fine-Grained Visual Categorization (BMVC, 2021)

[논문링크](https://arxiv.org/abs/2107.02341)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2107feature.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 fine-grained visual recognition을 위한 feature fusion vision transformer(FFVT)를 제안함
> - FFVT는 low-level, middle-level, high-level의 tokens의 정보를 통합하여 classification에 사용함
> - 이를 위해 FFVT는 mutual atttention weight selection(MAWS)를 사용하여 각 레이어에서 중요한 tokens를 선택하여 최종 layer에서 함께 사용함
- 또한, FFVT는 일반적인 fine-grained dataset을 넘어 small-scale, ultra-fine-grained 데이터셋에서 검증됨

## 접근법
- ViT의 각 layer에서 patch selection이 이루어지며, 각 레이어에서 선택된 patch들은 마지막 Transformer layer에서 concat되어 사용됨
> - TransFG는 마지막 encoder 전에 patch selection을 진행하는 데 반해, FFVT는 각 encoder layer에서 모두 patch selection을 진행
- 각 encoder에서는 mutual attention weight selection module을 통해 patch selection을 진행함
> - Encoder의 attention score map에서 CLS token에 관한 두 벡터 (first vector of row and column axis, respectively)를 선택하고, pointwise multiplication을 통해 mutual attention weight를 계산
- 이후 mutual attention weight를 기준으로 Top-K개의 patch를 선택함

## 실험결과
- CUB, DOG에서 SOTA를 달성하였고, ultra-fine-grained dataset인 SoyCultivarLocal, CottonCultivar80에서도 SOTA 성능 달성

## 의견
- /