# End-to-End Object Detection with Transformers (ECCV, 2020)

[논문링크](https://arxiv.org/abs/2005.12872)

<p align="center">
    <img width="600" alt='fig1' src="../img/carion2020end.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 아주 단순화된 end-to-end detection pipeline을 제안함 (Detection Transformer, DETR)
> - DETR은 NMS, anchor generation과 같은 기존의 hand-designed 과정을 모두 제거함
- 방법의 핵심은 object detection을 direct set prediction 문제로 생각하는 것
> - Detection을 regression 및 classification으로 우회하여 접근하던 이전의 방식들과는 다름
> - Transformer 기반의 encoder-decoder 구조를 사용하여 모든 objects를 한번에 예측함
> - Bipartite matching 기반의 global set loss 적용

## 접근법
- Detection을 direct set predictions으로 고려하기 위해서는 두 가지 필수적인 요소가 필요함
> - (i) 예측된 boxes와 ground-truth boxes 간의 unique matching을 강제하는 set prediction loss
> - (ii) Objects의 set을 한꺼번에 예측하기 위한 적절한 아키텍처

### (1) Object Detection Set Prediction Loss
- DETR은 decoder를 통해 하나의 이미지에서 $N$개의 predictions로 구성된 set을 예측함
> - $N$은 주어진 이미지의 ground-truth objects 수보다 훨씬 크게 설정됨
- 예측된 objects와 ground-truth objects 간의 optimal한 bipartite matching을 생성하고 object-specific한 loss를 optimize함
> - Ground-truth objects로 구성된 set의 크기를 'no object'로 padding하여 $N$과 맞춰줌
> - Ground-truth set과 predicted set의 최적 bipartite matching을 찾기 위해 Hungarian algorithm을 사용함
> - 즉, predicted objects마다 unique한 ground-truth가 매칭
> - Matching cost는 class prediction과 bboxes 간의 유사도를 모두 고려함
> - 이러한 bipartite matching은 이전의 NMS 및 anchor generation을 대체할 수 있음
- Matching을 수행한 다음 각각의 pair에 대해 기존의 detection loss와 유사한 hungarian loss를 계산
> - Class prediction에는 negative log-likelihood loss가 적용됨
> - Bbox regression은 target bbox 좌표를 직접 예측하는 방법이 사용되며, regression loss에는 L1 loss와 함께 scale-invariance를 고려하기 위해 GIoU loss가 같이 사용됨

### (2) DETR architecture
- DETR은 매우 간단한 구조로 구성됨
> - (i) Feature 추출을 위한 CNN backbone
> - (ii) Encoder-decoder Transformer
> - (iii) 최종 예측을 위한 feed forward network(FFN)
- Backbone은 입력 이미지로부터 feature maps를 추출
- Output feature map에 1x1 convolution을 적용하여 channel을 줄이고 spatial dimension을 1차원으로 축소 (HxWxd -> HWxd)
- 이후 standard Transformer encoder 아키텍처로 이루어진 encoder layers를 통과 
- Decoder도 기존 Transformer decoder block을 사용하되, 기존의 decoder가 한 번에 하나의 output element를 예측하는 것과 달리 각 decoder block은 모든 $N$개의 예측을 동시에 처리함
- Decoder는 $N$개의 input embeddings(object queries)를 입력으로 받으며 encoder의 output과 attention을 진행
> - Self-, encoder-decoder attention을 통해 전체 이미지 context를 사용함으로써 모든 objects의 pair-wise relations를 모델링하고 global하게 추론함
- $N$개의 object queries는 decoder를 거쳐 output embeddings로 변환되며, 하나의 object embedding에 대해 3-layer MLP로 구성된 FFN을 사용하여 bbox coordinates와 class label을 각각 예측함
> - Bbox regression은 원본 이미지에 대해 nomarlized (x, y, w, h)를 예측
> - Class prediction은 $C$개의 classes와 bg class를 포함한 $(C+1)$개의 classes를 예측
- 학습 시 각 class 별로 예측되는 objects의 수를 조정하기 위해 각 decoder마다 같은 가중치를 공유하는 예측 FFN과 hungarian loss를 배치(auxiliary decoding losses)

## 실험결과
- 많은 epochs로 훈련시켰을 때, MS COCO 벤치마크에서 Faster R-CNN보다 좋거나 거의 비슷한 성능 달성
> - Trasformer는 최적화를 위해 많은 epochs가 필요하다는 단점이 있음
- 연산량 측면에서 보면 DETR이, FPS에서는 Faster R-CNN이 좋은 성능을 보임
- 인코더의 layer 수가 늘어날 수록 성능이 증가
- 또한 DETR은 panoptic segmentation task에서도 좋은 성능을 보임 (versatile/extensible한 모델임을 증명)
- Encoder의 attention은 이미지 수준의 global한 attention을 통해 object instance를 분리하는 반면, decoder의 attention은 보다 지역적인 attention (head, legs) 등에 집중하여 object의 경계를 추론함

## 의견
- /