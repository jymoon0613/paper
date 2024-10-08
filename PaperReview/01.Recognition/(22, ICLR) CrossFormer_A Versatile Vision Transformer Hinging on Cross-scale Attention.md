# CrossFormer: A Versatile Vision Transformer Hinging on Cross-scale Attention (ICLR, 2022)

[논문링크](https://arxiv.org/abs/2108.00154)

<p align="center">
    <img width="1000" alt='fig1' src="../img/wang2021crossformer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 등장 이후 많은 발전을 이루었지만 아직도 성능을 제한하는 이슈가 존재함: 많은 vision tasks에서 매우 중요한 서로 다른 scale features 간의 상호작용이 불가능함
> - 이미지 패치들은 모두 동일한 scale로 임베딩됨
> - 임베딩된 패치의 scale은 layer를 지나면서 변하지 g않고 유지되거나 pooling 연산을 통해 커지기도 하지만, 같은 layer 내부에서는 변하지 않고 equal-scale을 유지함
> - 또한, 연산량을 줄이기 위해 self-attention 시 인접한 패치들이 일부 병합되어 계산됨
> - 이것이 일종의 cross-scale interaction이라고 볼 수 있지만 merging 연산은 각 패치가 지니고 있는 small-scale(fine-grained) features를 잃어버리게 하는 원인으로 작용하여 cross-scale attention이 불가능함
- 따라서, 본 논문에서는 ViT의 cross-scale interaction을 가능하게 하기 위해 새로운 embedding layer인 cross-scale embedding layer(CEL)과 self-attention module인 long short distance attention(LSDA)를 제안함
> - CEL: 서로 다른 scale의 kernel을 여러 개 사용하여 embedding을 수행하며 이는 단일 scale 패치를 사용하되 각각의 패치는 cross-scale features를 내포하도록 함
> - LSDA: 기존 self-attention을 수정하되 small-scale features를 보존하기 위해 patch merging을 진행하지 않음. self-attention을 short distance attention(SDA)과 long distance attention(LDA)의 두 부분으로 나눔. SDA는 인접한 패치끼리의 dependency를 생성하고 LDA는 서로 멀리 떨어진 패치끼리의 dependency를 생성함. LSDA는 이전의 연구들과 같이 기존 self-attention의 복잡도를 개선하지만 patch merging을 수행하지 않아 small-scale 및 large-scale features를 손상시키지 않으므로 cross-scale interactions가 가능함
- 또한, 고정된 이미지 혹은 패치 그룹 사이즈에서만 작동하는 relative position bias(RPB)를 여러 image/group size에서도 유연하게 적용될 수 있도록 하기 위해 두 이미지 패치의 상대적 거리를 입력으로 받아 그들의 position bias를 출력하는 dynamic position bias(DPB) 모듈을 추가로 제안함
- 이러한 기능들을 바탕으로 여러 vision tasks에 적용 가능한 CrossFormer 구조를 디자인함

## 접근법
- CrossFormer는 pyramid structure로 구성: 4개의 stage로 구성
- 각 stage는 CEL과 CrossFormer block으로 구성됨
- CEL은 각 stage에서 토큰 임베딩을 위해 사용됨
> - Stage 1의 CEL은 이미지를 입력으로 받아 4개의 서로 다른 kernel size로 patch를 생성함 (convolution layer 사용) 
> - 계산 복잡도를 고려하여 kernel의 크기가 커질수록 embedding dimension(output channel의 수)을 줄임
> - 각 kenel 사이즈마다 생성되는 패치의 수를 고정하기 위해 패딩을 사용하며 stride를 모두 일치시킴 (e.g. 4x4)
> - 각 결과를 concat하여 하나의 이미지 패치로 사용
- 각 CrossFormer block은 LSDA와 MLP로 구성됨
- LSDA는 SDA와 LDA로 구성됨
> - SDA는 먼저 모든 토큰들에 대해 $G\times{G}$의 인접한 토큰들끼리 묶음
> - LDA는 $S\times{S}$의 입력 토큰들에 대해 고정된 interval인 $I$로 토큰들이 샘플링됨
> - 그룹화가 완료되면 SDA와 LDA 모두 생성된 각자의 그룹 내에서 self-attention을 수행
> - LDA는 cross-scale embedding을 통해서 이점을 얻을 수 있음: LDA는 서로 인접하지 않은 토큰끼리의 dependency를 모델링하므로 그 relationship을 판단하기 어렵지만, LDA를 통과하는 패치들은 CEL을 통해 multi-scale 정보를 포함하고 있으므로, 각 토큰에 내재되어 있는 large-scale 패치의 context 정보를 통해 dependency를 원활하게 생성할 수 있음
- Relative position bias는 한 토큰의 전체 seqeunce에서의 상대적인 위치를 모델링하기 위해 attention 수행 시 적절한 bias를 더해주는 기법
- 기존의 position biase matrix $B\in{R}^{{G^2}\times{G^2}}$는 고정된 크기의 matrix로 정의됨 (이미지 및 그룹의 크기가 $B$로 인해 제한됨)
- 따라서 relative position bias를 여러 image/group size에서도 유연하게 적용될 수 있도록 하기 위해 MLP 기반의 모듈인 DPB를 제안함
> - DBP는 i번째 토큰과 j번째 토큰의 좌표 거리를 입력으로 받아 3개의 MLP block을 거치고 최종적으로 i번째 토큰과 j번째 토큰의 relative position을 인코딩한 scalar를 출력함

## 실험결과
- Image classification, object detection, instance segmentation, semantic segmentation에서 모두 SOTA 달성

## 의견
- /