# Res2Net: A New Multi-Scale Backbone Architecture (IEEE T-PAMI, 2019)

[논문링크](https://ieeexplore.ieee.org/abstract/document/8821313)

<p align="center">
    <img width="400" alt='fig1' src="../img/gao2019res2net.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 여러 vision tasks에서 multi-scale features는 매우 중요하게 다루어져 왔음
- 본 논문에서는 여러 receptive field를 사용하여 multi-scale ability를 향상시키는 방법을 제안함 (Res2Net)
> - $n$개의 channels로 구성된 3x3 convolution filters를 $w$개의 channels로 구성된 더 작은 $s$개의 filters로 대체함
> - 이러한 filter groups는 residual connection을 통해 hierarchical하게 연결됨
- 제안된 Res2Net은 depth, width, cardinality와 함께 scale이라는 새로운 model dimension을 등장시키며, scale은 다른 모델 요소들보다 성능 향상에 더욱 중요함
- 또한, Res2Net을 구성하는 Res2Net module은 여러 기존 CNN에 쉽게 병합될 수 있음

## 접근법
- 1x1, 3x3, 1x1 convolution layer로 구성된 bottleneck block은 CNN의 가장 대표적인 building block으로 자리잡고 있음
- 본 논문은 bottleneck block의 computational load는 비슷하게 유지하면서 multi-scale ability를 개선할 수 있는 아키텍처 수정을 고려함
- 이에 본 논문은 bottleneck block의 3x3 convolution filters를 더 작은 여러 개의 filters로 나누고, 각각의 filter group은 hierarchical residual connection으로 연결됨
> - 입력 feature maps는 1x1 convolution을 거친 후 $s$개의 일정한 크기를 가지는 feature map subsets로 split됨
> - $s$개의 feature map subsets는 입력 feature maps와 동일한 크기를 지니지만 channel 수는 $1/s$가 됨
> - 첫번째 feature map subset($x_1$)을 제외한 feature map subset($x_i$)들은 각각 하나의 3x3 convolution($K_i$)에 대응됨
> - 두 번째 feature map subset($x_2$)의 output은 $y_2=K_2(x_2)$로 계산되며, $i=3$부터 output은 $y_i=K_i(x_i+y_{i-1})$로 계산됨
- 이러한 구성에서 마지막 3x3 convolution layer $K_s$는 모든 feature split을 볼 수 있으며, feature split들은 연속적으로 3x3 convolution을 거치는 과정에서 receptive field가 꾸준히 늘어남
- Feature split은 계속 더해지면서 3x3 convolution이 적용되기 때문에 Res2Net module은 서로 다른 receptive field sizes와 scales를 모두 포함하고 있는 output을 모델링할 수 있음 
- Multi-scale 정보를 더 효과적으로 fuse하기 위해 마지막으로 모든 feature split을 다시 concat하고 1x1 convolution을 적용함
- Res2Net module에서 feature split의 개수 $s$는 scale dimension을 control하는 parameter임
> - $s$가 커질수록 더 다양한 receptive field sizes를 가지는 features를 학습할 수 있음

## 실험결과
- ImageNet, CIFAR에서 모델을 평가함
- ResNet, Inception, ResNeXt 등 기존의 CNN 아키텍처에 Res2Net module을 추가했을 때 모두 유의미한 성능 개선이 발견되었음
- Scale $s$를 증가시킬수록 성능이 증가하는 경향을 보였음
- 또한, 같은 scale $s$를 지니는 모델에 대해 cardinality $c$와 width $w$를 증가시켰을 때 성능은 증가하지 않았으며 이는 scale이 cardinality, width와는 또 다른 model dimension임을 의미함
> - Scale은 width, cardinality와 orthogonal함
- 동일한 baseline에 대해 scale, depth, cardinality를 각각 증가시켰을 때의 성능을 평가한 결과 scale의 증가로 인한 모델의 성능 향상 폭이 가장 컸음
- Detection, segmentation에서도 좋은 성능 기록

## 의견
- / 