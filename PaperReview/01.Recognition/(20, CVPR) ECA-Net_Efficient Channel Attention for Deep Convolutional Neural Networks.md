# ECA-Net: Efficient Channel Attention for Deep Convolutional Neural Networks (CVPR, 2020)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Wang_ECA-Net_Efficient_Channel_Attention_for_Deep_Convolutional_Neural_Networks_CVPR_2020_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/wang2020eca.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Channel attention의 도입은 deep CNN 모델의 성능 향상을 이끌었음
- 대표적인 channel attention 방법으로는 SE-Net의 squeeze-and-excitation(SE) block이 있음
- 하지만 SE block은 연산량이 매우 크고 모델 복잡도도 크게 증가시킨다는 문제가 있음
- 따라서, 본 논문에서는 더 효율적인 방식으로 channel attention을 수행하는 방법에 대해 탐구해보고자 함
- 구체적으로, 본 논문은 dimensionality reduction을 사용하지 않고 효율적인 방식으로 cross-channel interaction으 모델링하는 efficient channel attention(ECA) module을 제안함

## 접근법
- 저자들의 실험으로부터 channel attention 시 dimensional reduction을 수행하지 않는 것이 효과적인 channel attention 구현을 위해 중요하다는 것이 밝혀짐
- 또한, cross-channel interaction을 모델링하는 것도 중요하다고 밝혀졌지만, 이를 구현하기 위해서는 많은 파라미터 추가가 요구되며, 따라서 모델 복잡도 및 연산량 증가가 불가피함
- 따라서, 저자들은 1D convolution을 사용하여 이웃한 $k$개의  channels 간의 interactions를 모델링하는 방식을 구현함
> - Channel 수에 따라 interaction range(1D convolution의 kernel size)가 달라짐
- 즉, global average pooling을 수행하는 것까지는 SE block과 같으나, 이후에 1D convolution과 sigmoid function을 적용하여 channel attention weights를 계산함
- SE-Net과 같은 설정에서 SE block을 ECA block으로 대체한 ECA-Net을 설계함

## 실험결과
- Classification, detection, segmentation 모두에서 좋은 성능을 기록함

## 의견
- / 