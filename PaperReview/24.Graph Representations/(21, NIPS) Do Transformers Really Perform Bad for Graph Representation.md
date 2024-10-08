# Do Transformers Really Perform Bad for Graph Representation? (NIPS, 2021)

[논문링크](https://proceedings.neurips.cc/paper/2021/hash/f1c1592588411002af340cbaedd6fc33-Abstract.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/ying2021transformers.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transformer는 대부분의 task에서 좋은 성능을 보여주었지만 graph representations에 있어서는 거의 적용되지 못했음
- 본 논문에서는 standard Transformer 아키텍처를 사용하여 graphlevel prediction tasks에서 SOTA 성능을 기록할 수 있는 Graphormer를 제안함
- Graphormer의 핵심은 graph의 structural information을 모델에 적절히 결합시키는 것
> - Learnable vector로 node의 centrality를 모델링하여 self-attention 시 각 node의 node importance가 반영될 수 있도록 함
> - Learnable vector로 node 간의 spatial relation을 모델링하여 self-attention 시 node 간의 structural relation이 반영될 수 있도록 함

## 접근법
- 일반적으로 graph data에 대한 self-attention은 각 node 간의 semantic correlation을 계산함
- 하지만, 각 노드가 graph에서 얼마나 중요한지를 의미하는 node centrality는 측정하지 못하며, node centrality는 graph understanding에서 매우 중요함
- 이를 위해 Graphormer는 centrality encoding을 통해 각 노드에 node centrality를 할당함
> - Indegree와 outdegree를 의미하는 2차원의 실수 embedding vector를 할당함
> - Self-attention 이전에 centrality encoding을 각 노드에 더해줌
- 또한 graph의 structural information을 반영하기 위해 spatial encoding을 사용함
> - Graph의 structural information을 반영하기 위해서는 두 노드 간의 spatial relation을 측정해야 하며, 본 논문에서는 두 노드 간의 shortest path의 distance로 정의함
> - Self-attention 계산 시 spatial encoding을 bias로 더해줌
- Node뿐만 아니라 edge도 structural information을 포함하고 있는 경우가 많고, 따라서 edge encoding을 통해 graph의 structural information을 추가로 반영함
> - Edge features와 learnable embedding의 평균 dot-product 값으로 설정
> - Spatial encoding과 마찬가지로 self-attention 시 bias로 더해줌

## 실험결과
- OGB Large-Scale Challenge, MolPCBA, MolHIV, ZINC 등의 graph-level prediction tasks에서 평가한 결과 SOTA 성능을 달성함

## 의견
- /