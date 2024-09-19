# Dense Attention Fluid Network for Salient Object Detection in Optical Remote Sensing Images (IEEE T-IP, 2020)

[논문링크](https://ieeexplore.ieee.org/abstract/document/9292434)

<p align="center">
    <img width="700" alt='fig1' src="../img/zhang2020dense.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Salient object detection(SOD)는 주어진 이미지에서 가장 중요한 object를 식별하는 작업임
> - 먼저 background와는 구분되는 salient objects/regions를 찾음
> - 이후 찾은 salient objects/regions에 대한 pixel-wise saliency map을 생성함
- 본 논문에서는 optical remote sensing images (RSIs)를 입력으로 사용하는 SOD에 대해 논의함
- 기존에 SOD가 많이 적용되었던 natural scene images (NSIs)와 비교했을 때 RSIs는 다음과 같은 차이점이 있음
> - (i) salient objects의 scale 변동이 심함, (ii) salient objects의 orientation 변동이 심함 (전면, 후면 등), (iii) background가 매우 다양한 형태로 나타날 수 있으며 여러 환경 변수에 의해 영향을 받을 수 있음 (i.e., illumination intensity)
> - 이러한 차이점으로 인해 NSIs를 위한 SOD를 optical RSIs에 바로 적용할 수 없음
- 본 논문에서는 RSIs에 대해 SOD가 어려운 이유에 근거하여 솔루션을 제안함
- 먼저, RSIs에서 salient objects는 background의 개입(e.g., cluttered background, imaging shadows)으로 인해 정확한 localize가 힘듦
> - 이러한 문제를 해결하기 위해 attention moudels가 SOD models에 추가되기도 함
> - 하지만, 이러한 attentive 모델들은 서로 다른 feature levels에서 모두 독립적으로 attention maps를 생성하고, 이는 서로 다른 level의 attention results 간의 연관성을 고려하지 못하므로 suboptimal함
> - 따라서, 본 논문에서는 서로 다른 level의 attention modules를 명시적으로 연결하는 dense attention fluid (DAF) 구조를 제안함 (DAFNet)
- 다음으로, RSIs의 salient objects는 NSIs의 salient objects와 비교했을 때 더 복잡한 구조로 형성되어 있음
> - 이는 convolution operation의 local receptive field로 인핸 멀리 떨어진 spatial positions 간의 feature inconsistency가 발생하기 때문임
> - 따라서, 본 논문은 모든 pixel pairs 사이의 long-range semantic dependencies를 caputre하여 global context 정보를 활용하는 global conetext-aware attention (GCA) mechanism을 제안함
- 마지막으로, RSI SOD dataset은 하나밖에 없으며 데이터 수도 매우 제한적임 (ORSSD)
> - 따라서, 기존의 데이터셋을 보다 확장함 (EORSSD)

## 접근법
- VGG16을 backbone으로 사용함
- VGG16을 구성하는 5개의 convolutional blocks로부터 intermediate feature maps를 각각 추출함
- 추출된 features에 대해 모든 spatial locations의 long-range semantic dependencies를 모델링하는 GCA를 적용함
- GCA 모듈은 global feature aggregation (GFA)와 cascaded pyramid attention (CPA)의 두 가지 구성요소로 이루어져 있음
- 먼저 GFA는 pixel pairs의 global semantic relationships을 aggregate하여 feature alignment와 saliency patterns 간의 mutual reinforcement를 수행함
> - 각 intermediate feature maps에 대해 matrix multiplication을 적용하여 mutual influence를 계산하고, softmax를 추가로 적용하여 global context map을 생성함
> - Global context map의 각 entry는 두 pixel location의 dosine-distance에 기반한 feature similarity를 의미함
> - Global context map을 feature maps에 곱하여 calibration해주고, residual connection으로 feature maps를 추가로 연결함
> - Output feature maps는 global contextual dependencies를 포함하고 있으므로 모든 salient feature regions의 feature consistency를 유지하게 해줌
> - 또한, channel attention도 수행하여 더 compact한 feature representations를 생성할 수 있도록 함
- 다음으로 cascaded pyramid attention을 적용하여 점진적으로 features와 attentive cues를 coarse-to-fine 방향으로 refine함
> - 먼저 GFA의 output feature maps점진적으로 max-pooling을 적용하여 서로 다른 resolutions의 feature pyramid를 생성함
> - 이후 각 scale의 feature maps로부터 spatial attention maps를 계산함
> - 이때 spatial attention maps는 가장 작은 resolution의 attention maps로부터 상위 scale로 upsampling하여 계산됨
- 이러한 GCA 모듈을 바탕으로 각각의 GCA output을 dense하게 연결하는 attention fluid structure를 정의함
- 이후 decoding 과정을 통해 saliency maps를 예측함
> - Top-down 방식으로 생성된 features들을 통합해나감

## 실험결과
- ORSSD 데이터셋 및 확장된 EORSSD 데이터셋에서 모두 SOTA 성능 기록

## 의견
- / 