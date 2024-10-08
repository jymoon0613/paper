# Swin Transformer: Hierarchical Vision Transformer Using Shifted Windows (ICCV, 2021)

[논문 링크](https://openaccess.thecvf.com/content/ICCV2021/html/Liu_Swin_Transformer_Hierarchical_Vision_Transformer_Using_Shifted_Windows_ICCV_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/liu2021swin.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transformer 구조는 여러 vision tasks 중 특히 classification에서 좋은 성능을 보여줌
- 본 논문에서는 이전의 CNN과 마찬가지로 classification을 넘어 여러 vision tasks에 대한 general-purpose backbone으로서의 Transformer 구조를 제안하고자 함
- 여러 NLP tasks에서 Transformer가 general-purpose backbone으로 쓰이는 것과 다르게 vision tasks에서는 그렇지 못하는 이유는 NLP의 word tokens와 달리 visual elements는 매우 다양한 scale로 표현될 수 있다는 점이며, 이는 특히 detection과 같은 vision task에서 매우 중요한 문제임 
- 기존의 Transformer 기반 모델(ViT)의 경우 고정된 scale의 image tokens를 처리하기 때문에 다양한 scale을 고려하지 못했으며, 이는 scale에 대한 고려가 중요한 vision tasks에는 적용되기 힘들었음
- 또 다른 이유로는 텍스트 구절의 단어에 비해 이미지의 픽셀 resolution이 훨신 크다는 것임
- Self-attention의 연산량은 이미지 크기에 quadratic하기 때문에 Transformer는 high-resolution image에 대해 적용되기 힘들며, 이는 픽셀 수준에서의 조밀한 예측을 필요로하는 semantic segmentation과 같은 분야에 Transformer를 적용하기 힘들다는 것을 의미함
- 따라서, 본 논문에서는 hierarchical한 feature maps을 처리하며 연산량이 이미지 크기에 linear한 general-purpose backbone으로서의 Transformer인 Swin Transformer를 제안함
- Swin Transformer는 작은 크기의 이미지 패치에서 시작하여 처리가 진행될수록 점진적으로 인접한 패치들을 병합함
- 이러한 계층적 구조를 통해 Swin Transformer는 FPN과 U-Net을 사용할 때와 같이 조밀한 예측이 필요한 vision tasks(detection, segmentation)에 적용 가능함

## 접근법
### (1) Overview
- 먼저 입력 이미지는 4x4 크기의 패치로 분할되며 token embedding이 적용됨
- Embedding된 patch tokens은 Swin Transformer blocks를 통과함
- 이러한 Token embedding과 Swin Transformer blocks를 묶어 stage1으로 지칭함
- 이후의 stage에서는 token embedding이 patch merging으로 대체되어 stage가 진행될수록 feature map의 크기를 줄여 계층적인 구조가 만들어지도록 함
- Stage가 진행될 때마다 전체 patch 수는 1/4만큼 줄어들고, patch의 dimension (embedding dimension, channel은 2배씩 증가함)
- Swin Transformer block은 Transformer block의 기본적인 multi-head self-attention(MSA)을 shifted windows 기반의 MSA으로 바꾼 block임 (나머지는 같음)
- Patch embedding 과정에서 positional embedding을 사용하지 않고 self-attention 계산 과정에서 relative position bias를 더해줌

### (2) Shifted Window based Self-Attention
- MSA은 하나의 이미지 토큰이 다른 모든 이미지 토큰과 어떠한 관계에 있는지를 모델링함으로써 global context를 고려함
- 이러한 MSA는 토큰의 수(or iamge size)에 quadratic한 연산량을 유발하기 때문에 high-resolution에 기반한 pixel-wise dense prediction을 요구하는 vision tasks에는 적용하기 힘들었음
- 따라서, 효율적인 연산을 위해 self-attention을 local windows 내에 있는 패치끼리만 계산하는 방법을 제안함
- 하나의 window에 MxM개의 패치가 포함되도록 windows를 배정하고 window 내에서 self-attention을 수행하도록 하여 연산량을 줄임
- 하지만 local windows 기반의 self-attention은 window 간의 connections이 부족하기 때문에 representation power가 약함
- 따라서, local windows 기반의 self-attention의 연산량 이점을 이용하면서 windows 간의 교류를 활성화하기 위해, 연속적인 Swin Transformer block에서 두 가지 window partitioning 방식으로 번갈아가며 적용하는 shifted window partitioning approach를 사용함
- 먼저 첫 번째 Swin Transformer block에서는 일반적인 fixed local windows 기반의 self-attention을 적용함
- 다음 Swin Transformer block에서는 windows를 이동시켜서 self-attention 적용
- 이러한 shifted window partitioning approach은 이전 레이어의 fixed local windows 간의 정보를 서로 공유할 수 있도록 함

## 실험결과
- ImageNet에서 비슷한 파라미터 수의 CNN 및 Transformer 기반 모델의 분류 성능을 능가함
- ImageNet을 통해 학습된 Swin Transformer 모델을 Backbone으로 사용하였을 때, object detection, segmentation 쪽에서도 SOTA를 달성
- 학습 데이터가 많아질 수록 accuracy와 mAP가 증가하는 추세 (Transformer 계열 모델의 특징)

## 의견
- CvT와 마찬가지로 계층적 구조의 장점을 이용
- 실제 구현 코드 확인해보기