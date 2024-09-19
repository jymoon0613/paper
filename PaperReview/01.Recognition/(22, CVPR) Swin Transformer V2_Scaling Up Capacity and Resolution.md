# Swin Transformer V2: Scaling Up Capacity and Resolution (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Liu_Swin_Transformer_V2_Scaling_Up_Capacity_and_Resolution_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/liu2022swin.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- NLP에서는 scaling up된 language model (e.g. BERT (340M params))가 좋은 성능을 보여주는 데 반해 vision에서는 그 수준이 뒤쳐져 있음
> - 최근에서야 약 1M params로 scaling up된 케이스가 나타나고 있음
> - 현재의 large vision models는 오직 image classification task에만 국한됨
- 거대하고 general한 vision model을 훈련시키기 위해서는 몇 가지 이슈들을 해결해야 함
> - Large vision models는 학습 시 instability issue에 직면함
> - 많은 downstream vision task는 high-resolution input 혹은 large attention window를 필요로 하지만 low-resolution input으로 사전 학습된 모델을 high-resolution 모델로 효과적으로 전이하는 방법이 명확하지 않음
> - 추가적으로, resolution이 클 때 GPU 메모리 소모량도 문제가 됨
- 이러한 이슈들을 해결하기 위해 본 논문에서는 Swin Transformer를 기반으로 하는 몇 가지 기술을 제안함 (Swin Transformer V2)
> - 새로운 normalization 구성인 res-post-norm
> - 기존의 scaled dot product attention을 대체할 scaled cosine attention
> - Low-resolution input으로 사전 학습된 모델을 high-resolution input으로 효과적으로 전이하기 위한 log-spaced continuous position bias technique
> - GPU 메모리 소비량을 크게 절약하여 일반 GPU로 large vision 모델을 학습시킬 수 있는 여러 기술들

## 접근법
- Swin Transformer의 capacity 및 window 해상도를 확장하는 데 있어 두 가지 이슈를 발견
> - 모델 capacity를 확장 시 instability 문제 발생
> - 모델을 transfer할 때의 성능 저하: low-resolution에서 사전학습된 모델을 high-resolution에 전이하여 정확도 테스트하면 정확도가 크게 떨어짐
### (1) Scaling Up Model Capacity
- 기존 Swin Transformer는 일반적인 ViT와 마찬가지로 layer normalization을 각 블럭의 시작 부분에서 진행 (pre-normalization)
- 저자들은 이러한 Swin Transformer를 scaling up 했을 때 깊은 layer에서의 activation values가 심각하게 커지는 현상을 발견함
- 이는 pre-normalization 설정에서 각 residual block의 output activation values가 main branch로 바로 합쳐지고, 이는 깊은 layer로 갈수록 축적되어 커지기 때문임 (결과적으로 training instability를 유발)
- 이러한 문제를 해결하기 위해 residual post-normalization approach를 제안함
- Post-normalization 설정에서는 각 residual block의 output이 합쳐지기 전에 normalize되므로 더이상 깊은 layer로 갈수록 축적되지 않음
- 또한, 기존의 self-attention에서는 두 pixel pair의 similarity가 query vector와 key vector의 dot product로 계산되었는데, 저자들은 이러한 방식을 large visual model에 적용했을 때는 몇몇 pixel pairs가 전체 attention maps를 지배해버리는 문제가 자주 발생하는 것을 발견함 (특히 residual-post-normalization 사용 시)
- 따라서, 본 논문에서는 두 pixel pair의 similarity를 cosine similarity로 계산하는 scaled cosine self-attention을 제안함
### (2) Scaling Up Window Resolution
- 학습된 window size를 원활하게 확장시킬 수 있도록 relative position bias를 직접 최적화하는 것이 아닌 작은 네트워크를 통해 예측하도록 하는 continuous relative position bias 전략을 사용함
- Inference 시에는 각 relative position의 bias가 미리 계산되어 저장됨
- 매우 상이한 window size로의 transferring을 원활하게 하기 위해 상당히 많은 relative coordinate이 extrapolated되어야 함
- 이를 위해 기존의 linear-spaced 좌표 대신 log-spaced 좌표를 사용함 (extrapolation ratio가 크게 감소함)
### (3) Self-Supervised Pre-training
- Labeled data에 대한 의존을 줄이기 위해 self-supervised learning을 사용함
### (4) Implementation to Save GPU Memory
- 모델의 capacity와 resolution을 매우 크게 키우게 되면 GPU 메모리를 감당할 수 없는 문제가 생김
- 이러한 문제를 해결하기 위해 몇 가지 사항을 추가로 적용
> - Zero-Redundancy Optimizer (ZeRO) 사용
> - Activation check-pointing를 사용하여 기존에 feature map으로 인해 발생하던 메모리 문제 해결
> - Sequential self-attention computation 사용하여 self-attention을 batch 단위로 한번에 계산하는 것이 아닌 순차적으로 계산하도록 함

## 실험결과
- 제안하는 기법을 적용하여 30억개의 params를 가지는 Swin Transformer를 훈련시킬 수 있으며 1,536x1,536에 이르는 크기의 image resolution을 처리할 수 있는 모델 훈련 가능
- Image classification, object detection, semantic segmentation, video action classification에서 모두 SOTA 성능 달성

## 의견
- /