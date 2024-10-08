# Slide-Transformer: Hierarchical Vision Transformer with Local Self-Attention (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2304.04237)

<p align="center">
    <img width="400" alt='fig1' src="../img/pan2023slide.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT의 self-attention은 매우 큰 computation complexity를 수반함
- 이러한 문제를 해결하기 위해 여러 방법들이 제안되어 왔음
> - 전체 이미지에서 특정한 key/value positions에 대해서만 attention을 수행하는 sparse global attention(i.e. PVT, DAT) 
> - Attention 영역을 특정 local window 내로 제한하는 window attention(i.e. Swin Transformer, CSWin Transformer)
- 하지만 이러한 방법들은 여전히 몇 가지 문제에 의해 제한적임
> - Sparse global attention은 local features를 학습하는 데 취약하며, 일부 key/value를 선정하기 때문에 때로는 informative한 regions의 정보를 무시해버릴 수 있음
> - Window attention은 지정된 window 간의 communication이 잘 수행되지 않을 수 있고 window shifts를 위한 추가 디자인 요소들은 모델 구조를 제한할 수 있음
- 사용가능한 대안은 전통적인 convolution design과 마찬가지로 local attention을 수행하는 것
> - 각 query의 receptive field는 자신과 인접한 key/value로 제한됨
> - Local attention은 convolution의 translation-invariance, local inductive bias라는 장점은 취하면서, flexibility, data-dependency라는 self-attention의 장점은 유지할 수 있음
- 이전에도 CNN 혹은 Transformer에 local attention을 적용시키려는 시도가 있었지만, 이들은 inference time을 증가시키는 비효율적인 Im2Col function을 사용하거나 정교하게 설계된 CUDA kernels을 사용하여 구현되었음
- 따라서, high efficiency(inference time의 감소), high generalizability(CUDA support를 제공하지 않는 devices에도 적용 가능)를 수반하는 local attention module을 설계할 필요가 있음
- 따라서, 본 논문에서는 여러 ViT 모델에 효율적으로 통합될 수 있으면서 여러 하드웨어 디바이스와 호환되는 local attention module인 Slide Attention을 제안함

## 접근법
- Sparse global attention, window attention과 비교했을 때 local attention은 다음과 같은 장점을 지님
> - Query-centric attention pattern으로부터 local inductive bias를 획득할 수 있음
> - Convolution 연산과 마찬가지로 translation-equivariance를 얻을 수 있으며 이는 입력 shift에 대한 robustness를 보장함
> - Human design이 거의 필요 없으며 이는 모델 아키텍처에 대한 최소한의 제약만을 요구함
- 하지만 local attention은 실제 구현에 있어서 어려움이 있음
> - 피처맵의 각 query에 대한 receptive region이 모두 다르기 때문에 상응하는 keys/values를 각각 샘플링 해야함
- 이를 위해 Im2Col function은 각 쿼리에 중심이 맞추어진 window를 설정하고 해당 window 내의 region의 key/value를 샘플링함
- 이때 key/value matrix는 column 방향으로 구성됨
> - Key/value matrix의 HW개의 각 column은 query에 상응하는 key/value 값이 됨
> - 이러한 방식은 feature map을 독립적으로 slicing하는 방식으로 적용되기 때문에 data locality를 저해하며 수행 시간이 매우 김
- Im2Col function의 비효율성을 개선하기 위해 CUDA kernels를 설계하여 대체하는 방법이 제안되었지만, 이는 CUDA support가 가능한 하드웨어에서만 사용 가능하다는 점에서 한계가 있음
- 우선 Im2Col의 column 기반 matrix를 row 기반 matrix로 바꿀 수 있음
> - Key/value matrix의 $K^2$개의 각 row는 query에 상응하는 shifting input이 됨
> - 이러한 row-based view는 구현 방법에만 차이가 있고 결과로 나오는 key/value matrix는 기존의 Im2Col과 일치
- 이러한 변경을 기반으로 depth-wise convolution을 사용하여 샘플링 방식을 훨씬 효율적으로 설계할 수 있음
> - 기존의 feature map slicing 방식의 sampling에서 depth-wise convolution의 kernel weight을 조작하여 같은 연산을 간단한 convolution을 통해 구현
- 추가로 local attention의 flexibility를 향상시키기 위해 deformed shifting module을 제안함
- 즉, depth-wise convolution 기반의 현재 구현에 deformable convolution의 기능을 추가함

## 실험결과
- 여러 Transformer 모델에 통합하였을 때 baseline 성능을 모두 개선함
- Runtime(inference time)과 model performance trade-off에서 더 효과적인 모습을 보여주었음

## 의견
- /