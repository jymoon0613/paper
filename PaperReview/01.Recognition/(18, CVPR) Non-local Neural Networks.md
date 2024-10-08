# Non-local Neural Networks (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Wang_Non-Local_Neural_Networks_CVPR_2018_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/wang2018non.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝에서 long-range dependencies를 모델링하는 것은 매우 중요함
> - Language와 같은 sequence data의 경우 주로 recurrent operations를 통해 long-range dependency를 모델링함
> - Image data의 경우 convolutional operations를 여러 개 stack하여 large receptive field를 구현하는 방식으로 long-range dependency를 모델링함
- Convolution 및 recurrent operations는 모두 space 및 time의 관점에서 'local neighborhood'를 처리하는 방식임
> - Convolution 및 recurrent operations를 반복적으로 적용하여 long-range dependencies를 모델링할 수 있음
- 하지만, 이러한 local operations를 반복적으로 적용하는 방식은 여러 한계가 존재함
> - 계산 측면에서 비효율적임
> - Optimization 측면에서 어려움이 있음
> - 멀리 떨어진 positions 간의 정보 공유가 어려움 (multi-hop dependency modeling이 어려움)
- 본 논문에서는 long-range dependencies를 모델링하는 효율적이고, 간단하고, generic한 operations인 non-local operations를 제안함
> - Non-local operation은 각 position의 feature response 계산하기 위해 feaute map 상의 모든 position을 고려함
> - 이떄 position은 space(image), time(sequence), spacetime(video) 모두 가능함
- Non-local operations는 다음과 같은 장점이 있음
> - 반복적으로 연산을 적용해야 하는 local operations인 convolution 및 recurrent operatios와 달리, non-local operations는 두 positions 사이의 interactions를 바로 계산하여 long-range dependencies를 모델링할 수 있으며, 이때 두 positions 사이의 거리는 고려될 필요가 없음
> - Non-local operations는 효율적이며 매우 적은 layers만으로도 좋은 성능을 기록할 수 있음
> - Non-local operations는 다양한 input size를 처리할 수 있으며 다른 operations(e.g., convolution)와 쉽게 결합될 수 있음
- 본 논문은 video classification task에서 non-local operations를 적용함
> - Viedo에서 long-range interactions를 space와 함께 time 측면에서도 고려되어야 함 (spacetime)

## 접근법
- Non-local operation은 input signal $\mathbf{x}$를 같은 크기의 output signal $\mathbf{y}$로 매핑함
- 이때 output signal의 특정 position $\mathbf{y}_i$는 input signal의 해당 position $\mathbf{x}_i$와 자신을 포함한 input signal의 모든 positions $(\forall{j})\mathbf{x}_j$의 interactions에 기반하여 계산됨
> - Interaction은 function $f$와 $g$에 의해 계산됨
> - $f(\mathbf{x}_i, \mathbf{x}_j)$는 $\mathbf{x}_i$와 $\mathbf{x}_j$ 간의 relationship을 계산하여 scalar로 출력함
> - $g(\mathbf{x}_j)$는 $\mathbf{x}_j$의 representation을 계산하여 출력함
> - Input signal의 모든 position $j$에 대해 $f(\mathbf{x}_i, \mathbf{x}_j)g(\mathbf{x}_j)$을 계산하여 summed up하고, 결과를 normalize하여 $\mathbf{y}_i$를 구함
- Non-local operation은 input signal의 모든 positions $\forall{j}$를 고려하므로 local neighborhood만을 고려하는 convolution 및 recurrent operations와 차이가 있음
- 또한, non-local operation은 서로 다른 positions의 relationships를 고려하여 response를 계산하므로 단순히 weight를 학습하는 fully-connected(FC) layer와는 다름
> - 추가로, 고정된 input/ouput size를 필요로하는 FC layer와 달리 non-local operation은 여러 input size를 유연하게 처리할 수 있으며, output size는 input size와 동일하게 유지됨
- Non-local operation의 functions $f$와 $g$는 여러 방식으로 구현할 수 있으며, 어떤 버전을 선택하든 성능에 큰 차이는 없었고, 이는 non-local behavior 자체가 성능 향상의 근본적인 이유라는 것을 의미함
- 우선, 본 논문에서는 $g$를 linear embedding(1-layer MLP)로 고정함
> - 1x1(image) or 1x1x1(spacetime) convolution layer를 통해 구현함
- Pairwise function $f$의 다양한 버전을 탐구해봄
> - (i) $f$를 $\mathbf{x}_i$와 $\mathbf{x}_j$의 dot-product similarity를 계산하는 Gaussian function으로 정의할 수 있음
> - (ii) 또한, Gaussian function을 확장하여 similarity를 embedding space에서 계산하는 embedded Gaussian function으로 $f$를 정의할 수 있음
> - 이때 $\mathbf{x}_i$와 $\mathbf{x}_j$는 각각 서로 다른 weights로 embedding된 후 similarity를 계산함 (self-attention과 매우 유사함)
> - (iii) 혹은 Gaussian function 대신 embedding된 $\mathbf{x}_i$와 $\mathbf{x}_j$의 단순 dot-product를 $f$로 사용할 수 있음
> - (iv) 마지막으로, embedding된 $\mathbf{x}_i$와 $\mathbf{x}_j$를 concat하여 하나의 weights로 projection하는 concat 기반의 $f$를 정의할 수 있음
- Non-local operation을 바탕으로 non-local block을 정의함
> - Non-local operation의 output $\mathbf{y}$를 linear projection한 뒤 input $\mathbf{x}$을 residual connection으로 연결하여 최종 output $\mathbf{z}$를 출력

## 실험결과
- Video classification을 위한 ResNet-50 기반의 2D CNN, 3D CNN 베이스라인을 정의하고, 각각에 non-local blocks를 추가한 counterparts도 정의하여 실험함
- 2D CNN 베이스라인과 비교헀을 때 Non-local blocks가 추가된 2D CNN은 training 및 validation 측면에서 모두 더 뛰어났음
- Pairwise function $f$의 경우 여러 variations에 대해 거의 유사한 성능을 보여주었음
> - Embedded Gaussian을 디폴트로 사용하도록 설정함
- 베이스라인에 non-local blocks를 많이 추가할수록 일반적으로 성능이 더 향상됨
- 또한, 5개의 non-local blocks를 추가한 ResNet-50 모델은 더 깊은 ResNet-101 베이스라인보다 좋은 성능을 기록하였으며, 이는 non-local block의 추가가 단순히 모델 depth 증가로 인한 성능 향상을 이끄는 것은 아님을 증명함
- 3D CNN 베이스라인에 non-local blocks를 추가했을 때도 베이스라인에 비해 성능 향상이 있었음
- 훨씬 긴 frame 길이(기존의 32-frame에서 128-frame으로 변경)도 구성된 video input을 사용했을 때도 non-local blocks의 추가는 베이스라인의 성능 향상을 이끌었음
- Mask R-CNN에 non-local blocks를 추가하는 경우 detection 및 segmentation에서도 유의미한 성능 개선이 있었음

## 의견
- / 