# DINOv2: Learning Robust Visual Features without Supervision (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2304.07193)

<p align="center">
    <img width="800" alt='fig1' src="../img/oquab2023dinov2.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 self-supervised learning이 대규모 curated data로부터 'all-purposed visual features'를 학습할 수 있는지를 탐구하고자 함
> - 모델과 data size를 scaling 했을 때 self-supervised learning을 안정화시키고 가속화시키는 법에 집중
> - Uncurated images를 필터링하고 rebalance하는 파이프라인에 관에서도 다룸
- 결과적으로, 서로 다른 ViT로 훈련시킨 pre-trained visual 모델인 DINOv2를 도출함

## 접근법
- ImageNet, Google Landmarks를 포함한 여러 데이터 sources로부터 curated images를 수집하고, 웹 크롤링을 통해 대규모 uncurated images도 수집
- 이후 copy detection pipeline을 적용하여 거의 중복되는(near-duplicate) images를 제거
- Uncurated images에 대해 curation을 수행
> - Pre-trained ViT-H/16 모델로 curated, uncurated images로부터 embeddings를 모두 추출
> - Uncurated image embeddings에 K-means clustering을 수행
> - 각 curated image에 대해 가장 가까운 $N$개의 uncurated images를 선택하거나, 가장 가까운 cluster에 속하는 $M$개의 uncurated images를 선택
- 위 data processing 과정으로 총 142M개의 images로 구성된 데이터셋 획득(LVD-142M dataset)
- 이후 LVD-142M에 대해 self-supervised pre-training 수행
- Self-supervised method는 DINO, iBOT, SwAV 등과 여러 추가적인 기법들을 섞어서 설계
> - DINO 아키텍처로 image-level representations 학습
> - Student 모델 입력 시 패치 일부를 마스킹하여 patch-level representations 학습
> - Image-/patch-level representations 학습에서 사용되는 head는 공유되지 않음
> - SwAV의 Sinkhorn-Knopp centering을 teacher Softmax-centering 대신 사용
> - KoLeo regularizer 사용
> - Pre-training 과정 후반부에는 518x518의 큰 resolution 사용
- Efficient implementation을 설계하여 같은 하드웨어로 2배 빠르고 3배 적은 메모리를 사용하도록 함
> - FlashAttention을 사용하여 self-attention layers의 메모리 사용과 속도를 개선 
> - Self-attention시 nested tensors를 사용하여 서로 다른 패치 수를 가지는 global crops와 local crops를 동시에 처리
> - 샘플링을 통해 residual connection을 랜덤하게 제거하는 효율적인 stchastic depth를 사용
> - 전체 GPU 메모리를 최대로 활용하는 fully-shared data parallel(FSDP) 사용
> - 처음에는 가장 큰 모델 버전(ViT-g)를 학습하고, 이후 작은 모델 학습에는 해당 모델을 teacher로 사용

## 실험결과
- 대부분의 경우에서 curated images로 훈련하는 것이 uncurated images로 훈련하는 것보다 성능이 좋았고, 이는 self-supervised learning을 사용하더라도 curated data가 중요하다는 것을 의미함
- 반면 LVD-142M으로 훈련시켰을 때 성능이 가장 좋았고 이는 curated data가 더 diverse해질수록 학습되는 feautues의 퀄리티가 증가한다는 것을 의미함
- Image/video classification, instance recognition, segmentation, depth estimation 등에서 기존 self-supervised learning 혹은 weakly-supervised learning(text 등 image 이외의 추가 signals를 사용하여 훈련)보다 크게 개선된 성능을 기록
- 시각화 결과 DINOv2로 학습된 모델은 object와 background를 잘 구분하고 있었고, object의 각 part도 같은 카테고리에 속하는 images 간에 서로 유사하게 나뉘어지는 성질을 보여 semantic 정보를 충분히 학습한 것으로 보임
- 또한, 서로 다른 objects라 할지라도 유사한 parts에 대한 matching이 잘 이루어졌으며 이는 DINOv2로 훈련된 모델이 여러 domains에 걸친 generalizable한 semantic 정보를 학습한 것으로 보임

## 의견
- /