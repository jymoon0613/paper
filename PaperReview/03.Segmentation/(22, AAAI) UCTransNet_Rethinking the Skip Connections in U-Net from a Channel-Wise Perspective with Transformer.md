# UCTransNet: Rethinking the Skip Connections in U-Net from a Channel-Wise Perspective with Transformer (AAAI, 2022)

[논문링크](https://ojs.aaai.org/index.php/AAAI/article/view/20144)

<p align="center">
    <img width="800" alt='fig1' src="../img/wang2022uctransnet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- U-Net은 medical image segmentation에 가장 널리 쓰이는 encoder-decoder 구조 기반의 모델임
> - Encoder는 low-level 및 high-level features를 포착하며, decoder는 그러한 semantic features를 결합하여 최종 예측을 수행함
- U-Net의 skip connection은 decoder의 reconstruction 과정에서 encoder의 downsampling 과정에서 손실되는 정보를 보완하도록 encoder layer의 features를 decoder에 전달하는 역할을 함
- 하지만, 저자들이 실험을 진행한 결과 skip connection을 사용하더라도 encoder와 decoder 간의 semantic gap을 고려하지 않는다면 global multi-scale context가 잘 모델링되지 않는다는 한계가 있었음
> - Skip connection에 대한 실험을 진행한 결과, 일부 데이터셋에서는 skip connection을 사용하지 않을 때 성능이 더 좋았던 경우가 있었고, 이는 skip connection이 항상 좋지는 않다는 것을 의미함
> - 또한, 모든 encoder layer와 decoder layer를 연결하는 것이 항상 좋은 것은 아니였으며, 일부 layer의 연결이 다른 layer의 연결보다 매우 중요한 경우도 있었음
- 즉, 단순한 skip connection만을 사용하는 것이 아니라 feature fusion을 위한 효과적인 방법을 찾을 필요가 있음
- 하지만 이를 위해서는 두 가지 고려해야 할 사항이 있음
> - Multi-scale features의 효과적인 결합을 통해 global contexts를 효과적으로 모델링하려면 어떤 encoder features가 decoder에 연결되어야 하는지
> - 그렇다면 어떻게 semantic gaps를 고려하면서 features를 결합할 수 있는지
- Semantic gaps는 두 가지 양상으로 나타남
> - Encoder와 decoder stages 간의 semantic gaps
> - Multi-scale encoder features 간의 semantic gaps
- 따라서, 본 논문에서는 이러한 문제점을 해결하기 위해 기존의 skip connection design을 변경하여 encoder, decoder stages 간의 features를 연결시키는 개선된 방법을 제안함 (UCTransNet)

## 접근법
- 제안하는 UCTransNet은 vanilla U-Net의 encoder와 decoder 간의 연결을 기존 skip connection이 아닌 channel-wise Transformer module로 대체함
> - 구체적으로, skip connection은 Channel Transformer(CTrans)로 대체함
> - CTrans는 multi-scale encoder features를 fuse하기 위한 channel wise cross fusion transformer(CCT)와 향상된 encoder features를 decoder features와 fuse하는 channe-wise cross attention(CCA)로 구성됨 
- CCT는 multi-scale feature embedding, multi-head channel-wise cross attention, multi-layer perceptron(MLP)로 구성됨
> - Multi-scale feature embedding은 4-stages의 skip connection outputs 각각을 token embedding함
> - Multi-head cross-attention은 생성된 4개의 tokens에 대해, 각 stage의 token과 전체 stage tokens 간의 cross-attention을 계산함 (이후 MLP 통과)
> - 기존의 self-attention과 다른 점은 patch-axis가 아닌 channel-axis에 대해 self-attention을 수행한다는 점
- Encoder features와 decoder features의 semantic gaps를 해소하면서 연결하기 위해 CCA를 사용함
> - 4개의 CCT output과 대응되는 4개의 decoder feature map에 CCA를 적용함
> - 먼저 CCT output과 decoder feature map 각각에 global average pooling을 적용함
> - 이후 각각에 linear layer를 적용하여 transform하고 더해줌 (channel-wise dependency를 모델링)
> - 결과 값에 sigmoid를 적용하여 channel attention map을 만들어주고, decoder output에 곱해서 channel attention을 수행함
> - Masked된 output이 up-sampling된 features와 결합되어 전달됨

## 실험결과
- 기존의 medical image segmentation 성능을 개선함

## 의견
- /