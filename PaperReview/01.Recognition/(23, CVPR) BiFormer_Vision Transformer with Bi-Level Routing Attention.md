# BiFormer: Vision Transformer with Bi-Level Routing Attention (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2303.08810)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhu2023biformer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 computer vision community에서 많은 발전을 이끌었으며, 그 중 유명한 topic은 Transformer block의 attention module임
> - Transformer의 self-attention module은 CNN과 달리 global receptive field를 가지며 long-range dependency를 모델링할 수 있다는 장점이 있음
- 하지만 이러한 attention module은 모든 spatial location에 걸치 pairwise attention을 수행한다는 점에서 computational cost가 매우 큼
- 이러한 문제를 해결하기 위한 방법 중 하나로 key-value pairs의 일부에 대해서만 attention을 수행하는 sparse attention이 해결책이 될 수 있음(e.g., Swin Transformer, MaxViT)
- 이러한 접근법들은 다양한 방식으로 key/value를 merge하거나 select하지만 이는 query-agnostic하다는 문제가 있음 (merge 및 select된 tokens이 모든 query에 대해 공유됨)
- 하지만, ViT 및 DETR의 시각화 결과를 보면 서로 다른 semantic regions의 query들은 서로 다른 key-value pairs에 attend하고 있었음
> - 즉, 모든 query가 같은 key/value 집합에 attend하도록 하는 것은 적합하지 않음
- 따라서, 본 논문에서는 dynamic한 query-aware sparsity를 고려하는 attention mechanism을 제안하고자 함
> - 각 query가 가장 semantically relevant한 key-value pairs에만 attend하도록 함
- 하지만 어떻게 각 query마다 key-value pairs를 결정할 것인가에 대한 문제가 있음
- 본 논문에서는 효율적이면서 global하게 valuable한 key-value pairs를 locate할 수 있는 region-to-region routing approach를 제안함 (Bi-level Routing Attention, BRA)
> - Fine-grained한 token level이 아닌 coarse-grained한 region level에서 가장 상관이 적은 key-value pairs를 제거함
> - 이는 먼저 region-level affinity graph를 만들고, 각 노드별로 top-k개의 connections만 유지되도록 pruning함으로써 구현이 가능함
> - 즉, 각 region은 오직 top-k개의 routed region만 attend하면 됨
> - Attending region이 결정되면 token-to-token attention이 적용됨
- BRA로 구성된 block을 쌓아 BiFormer 구조를 설계함

## 접근법
- 먼저 입력 2D feature map을 SxS의 non-overlapped region으로 reshape함 (${H}\times{W}\times{C}$ -> ${S^2}\times{\frac{HW}{S^2}}\times{C}$)
- Linear projection을 통해 query, key, value matrix 생성
- 다음으로는 attending relationship을 도출
> - 먼저 query, key matrix에 per-region average를 적용하여 reigon-level queries와 keys를 도출함 ($Q^r, K^r\in{R^{{S^2}\times{C}}}$)
> - 이후 $Q^r, K^r$의 matrix multiplication을 통해 $R^{{S^2}\times{S^2}}$의 adjacency matrix $A^r$을 도출함
> - $A^r$의 각 entry는 두 region이 semantic하게 얼마나 연관되어 있는지를 계량함
> - $A^r$에 row-wise top-k를 적용하여 pruning을 수행함 ($I^r\in{N^{{S^2}\times{k}}}$)
> - $I^r$의 i번째 row는 i번쨰 region과 가장 연관있는 k개의 region의 index를 담고 있음
- region-to-region routing index matrix $I^r$을 바탕으로 fine-grained token-to-token attention을 수행
> - i번째 region에 속하는 각 query token은 $I^r_{(i,1)},\dots,I^r_{(i,k)}$의 모든 key-value pairs와 attention을 수행
> - 효과적인 연산 수행을 위해 $I^r$에 대해 먼저 key와 value matrix를 gather함
- BRA를 기반으로한 BiFormer 아키텍처 제안
> - 4-stage의 pyramid 구조로 구성됨
> - SwinT처럼 각 stage의 첫 단계에서 patch merging을 수행

## 실험결과
- 여러 모델 사이즈에서 BiFormer는 SOTA image classification 성능을 기록
- Object detection, instance segmentation, semantic segmentation에서도 좋은 성능을 기록함
- 다른 sparse attention mechanisms(i.e., deformable, cross-shaped window)와 성능 비교했을 때도 가장 좋은 성능을 보였음

## 의견
- /