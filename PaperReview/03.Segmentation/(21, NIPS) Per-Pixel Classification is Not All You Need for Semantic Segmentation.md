# Per-Pixel Classification is Not All You Need for Semantic Segmentation (NIPS, 2021)

[논문링크](https://proceedings.neurips.cc/paper/2021/hash/950a4152c2b4aa3ad78bdd6b366cc179-Abstract.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/cheng2021per.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Per-pixel classification은 이미지를 pixel 단위로 classification하여 하나의 이미지를 서로 다른 class를 지니는 regions로 분할함
- 반면 mask classification은 segmentation을 수행하기 위해 image partitioning과 classification 과정을 분할함
> - 각 픽셀을 classification하는 대신 binary masks의 집합을 예측하고, 이때 각 binary mask는 하나의 class prediction에 해당됨
> - Mask classification은 보다 flexible하며 주로 instance-level segmentation에서 사용됨 (i.e., Mask R-CNN, DETR)
- 본 논문은 mask classification이 semantic 및 instance segmentation tasks 모두를 위한 general한 solution이 될 수 있다고 주장함
- 따라서, DETR의 set prediction 매커니즘을 사용하는 간단한 mask classification 모델인 MaskFormer를 제안함
> - MaskFormer는 semantic/instancel segmentation을 어떠한 구조/loss/훈련과정 변경도 없이 unified하게 처리할 수 있음

## 접근법
- Per-pixel classification은 이미지의 모든 픽셀에 대해 class label을 예측함
> - 간단하고 직관적임
- Mask classification은 segmentation task를 image partitioning과 classification 과정으로 분할함
> - 이미지를 binary mask의 형태로 $N$개의 regions로 분할함
> - 이때 각 region이 $K+1$($K$개의 object classes + no object class)개의 classes 중 하나의 class로 배정됨
> - 예측된 masks/classes를 ground-truth masks/classes와 비교하여 loss를 계산함
> - DETR과 마찬가지로 bipartite matching을 사용함
- MaskFormer는 3개의 모듈로 구성됨: pixel-level module, Transformer module, segmentation module
- Pixel-level module은 입력 이미지로부터 per-pixel embeddings를 추출함
> - Backbone은 입력 이미지로부터 low-resolution feature map을 출력함
> - Pixel decoder는 backbone의 output feature map을 점차적으로 upsampling하여 per-pixel embeddings를 생성함(입력 이미지 resolution과 동일한 resolution으로)
- Transformer module은 Transformer decoder를 사용하여 $N$개의 per-segment embeddings를 계산함
> - Transformer decoder는 $N$개의 learnable positional embeddings(queries, DETR과 동일)를 입력으로 받음
> - $N$개의 queries와 backbone의 output feature map을 통해 $N$개의 per-segment embeddings를 출력
- Segmentation module은 per-segment embeddings으로부터 $N$개의 binary masks 및 class prediction을 수행함
> - Per-segment embeddings에 linear classifier를 부착하여 $N$개의 segment에 대해 각각 class prediction을 수행함
> - 또한, per-segment embeddings에 2-layer MLP를 부착하여 $N$개의 mask embeddings를 각각 생성함
> - $N$개의 mask embeddings 각각을 per-pixel embeddings와 dot product한 뒤 sigmoid activation을 적용하여 $N$개의 binary mask를 예측함
- Inference 시에 각 pixel은 예측된 $N$개의 (class)probability-mask pairs 중 가장 높은 score를 갖는 pair로 할당됨
> - Semantic segmentation task에서 같은 class label을 할당받은 segments는 merge됨
> - Instance segmentation task에서는 $N$개의 probability-mask pairs 중 어떤 pair가 할당되었는지를 의미하는 인덱스 $i$가 같은 class의 서로 다른 instance 구분을 위해 이용됨 

## 실험결과
- Semantic/instance segmentation 모두에서 좋은 성능을 기록함

## 의견
- /