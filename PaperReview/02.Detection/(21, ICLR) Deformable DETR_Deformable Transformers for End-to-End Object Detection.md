# Deformable DETR: Deformable Transformers for End-to-End Object Detection (ICLR, 2021)

[논문링크](https://arxiv.org/abs/2010.04159)

<p align="center">
    <img width="500" alt='fig1' src="../img/zhu2020deformable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- DETR은 fully end-to-end object detector이며, anchor generation, NMS와 같은 어떠한 hand-crafted components도 사용하지 않음
- 하지만 DETR은 수렴을 위해 훨씬 긴 training epochs가 필요하고 small objects를 detecting하는 것이 어렵다는 단점이 있음
> - 대부분의 최신 detectors는 multi-scale features를 사용하지만 이는 DETR의 경우 연산량이 매우 커지는 문제를 초래함
- 이러한 문제들은 Transformer의 구조적 특징에서 기인함
> - 학습 초기에 Transformer의 attention module은 feature maps의 모든 pixels에 거의 uniform한 attention weights를 할당하고, 이러한 attention weights가 sparse한 핵심 locations에 집중되도록 하기 위해 긴 training epoches가 요구됨
> - Transformer의 attention 연산은 pixel 수에 quadratic한 복잡도를 지니고, 이는 multi-scale features, 특히 high-resolution features 처리에 있어서 문제가 됨
- 따라서, 본 논문에서는 DETR의 slow convergence 및 high complexity 문제를 해결할 수 있는 Deformable DETR 모델을 제안함
> - Deformable convolution의 sparse spatial sampling과 Transformer의 relation modeling capability를 조합함

## 접근법
- Deformable attention module은 feature maps 전체에 대해 attention score를 계산하는 것이 아니라 reference point 주위의 일부 key sampling points에 대해서만 attend함
> - 입력 query feature로부터 sampling offsets과 attention weights를 계산 (linear projection을 통해)
> - Reference point는 query 자신의 위치
> - Deformable attention module은 pixel 수에 linear한 복잡도를 지님
- Deformable attention module을 확장하여 multi-scale feature maps를 처리하도록 함
> - Multi-scale deformable attention은 일반 deformable attention과 거의 유사함
> - Deformable attention이 single-scale feature maps에서 $K$개의 points를 샘플링했다면, multi-scale deformable attention은 $L$개의 multi-scale feature maps에 대해 확장되어 $LK$개의 points를 샘플링함
- Deformable Transformer encoder는 DETR의 Transformer encoder의 attention module을 제안된 multi-scale deformable attention module로 대체함
> - Deformable Transformer encoder의 입력과 출력은 모두 multi-scale feature maps
- Deformable Transformer decoder는 $N$개의 object queries를 입력으로 받음
- 이때 decoder의 cross-attention module을 multi-scale deformable attention module로 변경함
- Deformable DETR은 여러 변형을 적용하기 쉬움
> - 매 decoder layer에서 이전 decoder layer의 예측 결과를 바탕으로 bounding boxes를 refine하는 iterative bounding box refinement 전략을 사용하여 detection performance를 향상시킬 수 있음
> - 또한, deformable DETR을 2-stage로 구성할 수도 있음
> - 기존의 DETR에서, object queries는 이미지에 irrelevant한 decoder 입력임
> - 이때 먼저 region proposals를 수행하고, region proposals를 object queries 형식으로 decoder에 입력하는 2-stage 방식을 설계할 수 있음

## 실험결과
- DETR과 비교했을 때 deformable DETR은 10배 적은 training epochs로 더 좋은 성능 기록
> - Iterative bounding box refinement 혹은 2-stage 방식을 사용하면 성능을 더 높일 수 있음
- Multi-scale features를 사용하는 것은 small object에 대한 detection performance를 개선하는 데 효과가 있었음

## 의견
- /