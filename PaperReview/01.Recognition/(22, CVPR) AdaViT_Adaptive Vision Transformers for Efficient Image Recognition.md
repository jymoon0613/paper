# AdaViT: Adaptive Vision Transformers for Efficient Image Recognition (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Meng_AdaViT_Adaptive_Vision_Transformers_for_Efficient_Image_Recognition_CVPR_2022_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/meng2022adavit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Multi-head self-attention을 기반으로한 ViT는 CNN을 넘어서는 좋은 성능을 보였지만 패치 수에 quadratic한 computational cost가 발생한다는 문제가 있었음
- 하지만 모든 이미지가 많은 패치 연산과 self-attention layer를 필요로 하는 것은 아님
> - Object shape, size 등에 의해 이미지에는 large variations가 존재
> - Occlusion 등에 의해 복잡한 이미지에 대해서는 많은 패치 수와 self-attention block을 통한 충분한 contextual information 파악과 전체 이미지 수준에서의 이해가 요구됨
> - 쉬운 이미지에 대해서는 비교적 적은 수의 informative 패치만이 필요하며 attention head/block의 수도 적은 수를 유지해도 됨
- 따라서, 본 논문에서는 입력 이미지에 따라 어떤 패치가 사용되고, 어떤 self-attention heads/blocks가 활성화되는지를 적응적으로 결정하는 프레임워크를 제안함 (Adaptive Vision Transformer, AdaViT)
> - 이는 패치, head, block의 사용 여부를 binary하게 결정하는 가벼운 decision network를 각 block에 추가함으로써 구현할 수 있음

## 접근법
- Decision network는 patch selection, head slection, block selection policies를 결정하는 세 개의 linear layers로 구성됨
- 각 세 개의 linear layer를 거친 후 sigmoid가 적용됨
- Keeping/discarding에 대한 결정은 inference phase에서 thresholding을 통해 구현됨
- 하지만 서로 다른 샘플에 대해 optimal한 threshold를 찾는 것은 어려우므로 각 block마다 random decision variable을 정의하여 훈련시킴
> - Gumbel-Softmax trick을 통해 trainable(differentiable)하게 구현
- Patch selection은 가장 informative한 patch embeddings를 선택하도록 함
- Head selection은 각 attention head의 사용 여부를 결정함
> - Partial deactivation으로 구현하여 어떤 head가 사용되지 않아야 하면 identity matrix를 통해 attention 과정 없이 value만 그대로 output되도록 함
- Block selection은 self-attention, feed-forward network 각각에 대한 selection으로 구분함
- Objective function은 정확도를 고려하면서 computational cost가 최소가 되도록 함: usage loss and cross-entropy loss

## 실험결과
- 정확도의 근소한 하락으로 큰 효율성 획득

## 의견
- /