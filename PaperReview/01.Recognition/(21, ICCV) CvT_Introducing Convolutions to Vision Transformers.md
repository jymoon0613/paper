# CvT: Introducing Convolutions to Vision Transformers (ICCV, 2021)

[논문 링크](https://openaccess.thecvf.com/content/ICCV2021/html/Wu_CvT_Introducing_Convolutions_to_Vision_Transformers_ICCV_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/wu2021cvt.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Vision Transformer (ViT)는 NLP에서 주로 사용되던 Transformer 아키텍처만을 전적으로 사용하는 최초의 컴퓨터 비전 모델임 
- ViT는 Transformer 아키텍처를 다음과 같이 적용함: 
> - 하나의 이미지는 서로 겹치지 않는 N개의 이미지 패치(non-overlapping patches)로 분할됨 (각 패치는 NLP에서의 토큰 역할) 
> - 각 패치에 positional encoding을 더하여 어느 정도의 spatial information을 반영 
> - 이후 일반적인 Transformer layer를 반복적으로 거쳐, 분류를 위한 global relations를 학습 
- 대규모 데이터셋에서의 ViT의 좋은 성능에도 불구하고, 훈련 데이터셋의 크기가 작다면 비슷한 모델 크기의 CNN 보다는 낮은 성능을 기록 
- 이는 ViT가 이미지 처리에서 중요한 local structure 학습 능력이 없기 때문임 
- Local structure는 이미지의 2D 공간 상에서 서로 이웃하는 픽셀끼리 상당히 높은 상관관계를 지니는 것을 의미함
- 반면, CNN은 local receptive fields, shared weights, spatial subsampling 등의 구조를 통해 local structure를 자연스럽게 학습하게 되고, 추가로 일정 수준의 shift, scale, distortion에 대한 invariance도 지니게 됨으로써 이미지 처리에 적합함
- 또한 CNN은 계층적 convolution 구조를 통해 edge와 같은 low-level 피처부터 semantic pattern과 같은 high-level 피처에 이르기까지 local spatial context를 단계적으로 학습할 수 있음 
- 본 연구에서는 성능 향상 및 강건성 개선을 위해 ViT에 CNN의 convolution 개념을 도입한 Convolutional Vision Transformer (CvT)를 제안함 
- ViT에 두 가지 convolution 기반 연산인 Convolutional Token Embedding과 Convolutional Projection을 도입함 
- CvT는 Transformer와 CNN의 장점을 모두 이끌어낼 수 있음
> - CNN: receptive fields, shared weights, spatial subsampling
> - ViT: dynamic attention, global context fusion, and better generalization

## 접근법
### (1) Overview 
- CNN의 계층적 구조를 도입했으며 총 3단계로 구성되었고, 각 단계는 Convolutional Token Embedding과 Convolutional Transformer Blocks로 구성됨 

### (2) Convolutional Token Embedding 
- CNN처럼 multi-stage로 구성된 계층적 구조를 사용하여 low-level 피처 학습에서 high-level 피처 학습으로 이어지는 local spatial contexts를 모델링함
- 입력 이미지 혹은 token들을 2D grid 형태로 reshape한 token map에 convolution을 적용하여 새로운 토큰을 생성하고 Layer Normalization을 적용함
- 새로 만들어진 token map은 resolution이 줄어들며 채널의 수도 변함 (일반적으로 채널 수 증가함)
- Convolutional Token Embedding은 convolution의 파라미터 조절을 통해 token의 수, token의 feature dimension을 각 단계마다 조정할 수 있도록 함 
- 이를 통해, 각 단계마다 token sequence의 길이를 줄이면서 (token map의 resolution이 감소하므로) token의 feature dimension을 키울 수 있으며 (token map의 채널은 증가하므로), 이는 각 token이 CNN에서처럼 더 넓은 spatial region을 참조하면서 더 복잡한 시각적 패턴을 표현할 수 있도록 해주는 효과가 있음 

### (3) Convolutional Projection for Attention 
- Local spatial context를 추가적으로 모델링하고 계산 효율성을 증진시키기 위해 Convolutional Projection 사용함
- 각 토큰은 2D token map으로 reshape되고, Depth-wise Separable Convolution이 Convolutional Projection을 위해 적용됨 
- 만들어진 2D query, key, value map은 self-attention을 위해 다시 1D로 flatten됨
- 만약 Depth-wise Separable Convolution의 kernel size가 1x1이라면 기존 ViT의 linear projection과 같음
- 일반 convolution이 아닌 Depth-wise Separable Convolution을 사용하여 연산량 및 파라미터 감소
- 또한, stride를 적절히 조절하여 key와 valua의 token 수를 줄일 수 있고 이는 multi-head self-attention의 연산량을 감소시킬 수 있음
- Convolutional Token Embedding과 함께 Convolutional Projection은 Transformer에 local context structure를 모델링할 수 있는 자체적인 기능을 추가한 것과 마찬가지이므로 더 이상 Positional Embedding이 필요 없음 (제거해도 성능의 유의미한 감소 없었음)

## 실험결과
- ImageNet에서 검증함 
- 다른 Transformer 기반 모델과 비교했을 때 적은 파라미터와 연산량으로 더 좋은 성능 기록

## 의견
- Transformer와 CNN의 장점을 결합 