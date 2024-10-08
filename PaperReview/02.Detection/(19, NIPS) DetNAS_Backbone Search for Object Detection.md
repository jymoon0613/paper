# DetNAS: Backbone Search for Object Detection (NIPS, 2019)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2019/hash/228b25587479f2fc7570428e8bcbabdc-Abstract.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/chen2019detnas.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 효과적인 object detection을 위해 적절한 backbone network를 선정하는 것이 중요함
- 하지만 대부분의 object detection 모델들은 image classification을 위해 디자인된 네트워크 구조를 그대로 가져와 사용하고 있음
> - 두 tasks의 목적 차이는 suboptimal한 성능을 야기할 수 있음
- 하지만 hand-crafted한 방식으로 최적의 network 구조를 찾는 것은 매우 복잡함
- Neural architecture search(NAS)는 최근 image classification tasks에서 최적화된 모델 구조를 찾는 데 큰 공헌을 했음
- 하지만 detection model은 ImageNet에서 pre-trained된 backbone을 필요로 하며 이는 NAS를 object detection에 적용하는 것을 힘들게 하는 요인으로 작용함
> - Optimize가 어려움: main task는 detection이므로, pre-training accuracy를 기준으로 사용하기에는 부적합함
> - 비효율적임: 정확한 평가 기준을 위해 먼저 네트워크는 ImageNet에서 훈련된 이후 detection task에서 fine-tune되어야 하는데 이러한 과정은 매우 많은 연산량과 시간을 요구함
- 따라서, 본 논문에서는 detection task에서 NAS를 사용하는 방법을 제안함 (DetNAS)
> - One-shot NAS methods에서 착안하여 NAS의 weight training과 architecture search를 분리하고(2-stage) detection을 위한 NAS를 가능하도록 함
- DetNAS는 세 단계로 구성됨
> - (i) one-shot supernet을 ImageNet에서 pre-training시키고, (ii) one-shot supernet을 detection datasets에서 fine-tuning하고, (iii) evolutionary algorithm을 사용하여 훈련된 supernet에서 architecture search를 수행함
- 이러한 과정을 통해 찾은 detection 구조를 DetNASNet으로 명명함

## 접근법
- NAS란 가능한 architecture search space에서 정의된 validation loss를 최소화하는 모델 구조 및 가중치 집합을 찾는 작업임
- 하지만 detection tasks는 ImageNet에서 pre-train된 가중치를 요구하므로 architecture search 시 이를 고려해야 함
- One-shot NAS는 전체 가능한 architecture 집합인 superset을 같은 가중치를 공유하는 subset으로 나누고, 최적의 subset을 다음의 superset으로 하여 최적 architecture를 선정하는 방법임
> - 이때 각 subset에 속하는 아키텍처의 가중치는 공유되기 때문에 가중치는 고정된 상태에서 최적의 아키텍처(or subset)를 찾기만 하면 되므로 수행 시간이 크게 감소함
- 본 논문은 이러한 one-shot NAS의 아이디어를 사용하여 detection을 위한 NAS를 제안함
> - 초기의 superset에서 ImageNet pre-training시 validation loss가 최소가 되는 subset을 식별
> - 해당 subset을 superset으로 하여 detection task에 fine-tuning했을 때 validation loss가 최소가 되는 subset을 식별
> - 해당 subset을 superset으로 하여 최적의 아키텍처를 선정
- Architecture search step에서는 evolutionary algorithm을 사용
> - 트리 구조와 마찬가지로 root node로부터 가능한 각 node(population of networks)를 평가하며 최적의 leaf node(optimal architecture)로 향하는 single-path를 찾음
- Search space는 ShuffleNetv2 block에 기반함

## 실험결과
- 제안된 DetNAS로 찾은 detection 모델인 DetNASNet는 FPN 구조에 기반하고 있음
- DetNAS는 detection task에서 여러 hand-crafted 모델들보다 좋은 성능을 기록하였음
- ImageNet pre-training이 문제가 된다면 pre-training을 하지 않고 detection tasks에서 scratch로 훈련하여 NAS를 적용하는 방식이 대안이 될 수 있음
- 하지만 실험 결과로부터 pre-training을 NAS에서 고려하는 것이 더 좋은 모델 성능을 보장해주었음

## 의견
- / 