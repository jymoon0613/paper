# Scale-Aware Trident Networks for Object Detection (ICCV, 2019)

[논문링크](https://openaccess.thecvf.com/content_ICCV_2019/html/Li_Scale-Aware_Trident_Networks_for_Object_Detection_ICCV_2019_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/li2019scale.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Detection에서 scale variation에 효과적으로 대응하는 것은 핵심 이슈 중 하나임
- 따라서 이러한 large scale variation에 대응하기 위한 여러 방법들이 제안되어 왔음
> - Image pyramids, feature pyramids
> - Image pyramids는 모든 scale로 표현된 객체에 대해 전체 모델 수준에서 동등하게 처리할 수 있다는 장점이 있지만, inference time이 매우 길다는 단점이 있음
> - Featur pyramids는 image pyramids를 in-network 수준에서 구현하여 computation이 훨씬 적지만, 서로 다른 scale의 objects가 서로 다른 layer의 multi-level features 하에서 모델링되므로 feature inconsistency가 발생한다는 단점이 있음
- 본 논문은 image pyramids와 feature pyramids의 장점을 결합한 method를 제안하고자 함
> - 모든 scale의 features에 대해 동일한 representational power를 지니게 하되 효율적인 연산을 가능하도록 함
- 구체적으로, trident block을 사용하여 여러 개의 scale-specific feature maps를 생성함
> - Trident block은 여러 개의 branches로 구성되어 있으며, 각 branch는 같은 network structure와 parameters를 공유하지만 dilated convolution을 사용하여 서로 다른 receptive field를 지님
- 또한, scale-aware training scheme을 사용하여 각 branch가 주어진 scale range에 receptive field를 적절히 matching할 수 있도록 함
- 이러한 TridentNet은 다른 baseline에 비해 성능을 유의미하게 개선하였으며 inference speed는 차이가 거의 없었음

## 접근법
- 저자들은 선행 실험으로부터 서로 다른 scale의 objects에 대한 성능은 네트워크의 receptive fields에 의해 영향을 받았으며, 가장 적절한 receptive field는 objects의 scale과 강하게 연관되어 있음을 발견함
> - Receptive field를 키울수록 large object에 대한 성능이 증가하였지만 small object에 대한 성능은 감소함
> - 따라서 적절한 receptive field를 찾을 필요가 있음
- TridentNet은 ResNet-101 backbone의 일부 covlution blocks를 trident blocks로 대체하는 방식으로 설계됨
> - Trident block은 여러 개의 parallel branches로 구성되어 있으며 dilation rate을 제외하고 기존의 convolution block과 같은 structure를 공유함
> - 예를 들어, 기존의 bottlneck residual block은 1x1 conv, 3x3 conv, 1x1 conv로 구성된 여러 개의 branch로 변경될 수 있으며, 이때 각 branch의 3x3 conv는 서로 다른 dilation rate을 지니는 dilated convolution으로 디자인됨
> - 이때 각 branch는 dilation rate을 제외하면 동일한 structure이므로 가중치를 공유함
- 이러한 weight sharing은 다음과 같은 장점이 있음:
> - Multi-scale 모델링을 위한 parameters 증가가 없음
> - 모든 scale의 objects는 동일한 representation power로 모델링됨 (동일한 수준의, 동일한 parameters로 구성된 layers를 거쳐)
> - Trident block의 각 branch는 서로 다른 receptive로 여러 번 훈련되므로, 동일한 parameters가 서로 다른 scale range 하에서 여러 번 훈련될 수 있음
- 하지만 TridentNet은 scale mismatching으로 인해 일정 부분 성능이 하락하는 모습을 보였음
> - Scale mismatching이란 small objects가 큰 dilation rate을 갖는 branch를 통과하면서 생기는 문제를 의미함
- 따라서, 각 branch 별로 적절한 scale objects를 matching시켜줄 필요가 있음
- 이를 위해 scale-aware training scheme을 제안함
> - 각 branch에 valid scale range를 설정하여 training 시 region proposals와 ground truth boxes의 scale이 해당 valid range 내에 있는 것만 해당 branch에서 처리하도록 함
- Inference 시에는 NMS를 적용하여 각 brach에서 산출된 detection outputs를 combine하고 최종 결과를 얻음

## 실험결과
- COCO dataset에서 다른 methods보다 높은 성능을 기록함

## 의견
- / 