# PVANET: Deep but Lightweight Neural Networks for Real-time Object Detection (NIPS, 2016)

[논문링크](https://arxiv.org/abs/1608.08021)

<p align="center">
    <img width="600" alt='fig1' src="../img/kim2016pvanet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 가벼운 feature extraction network 아키텍처를 디자인하여 real-time object detection을 수행하고자 함 (PVANET)
- 핵심 디자인 아이디어는 feature channel 수를 줄이되 layers를 더 많이 사용하는 것임
- 또한, 이전에는 detection task에서 검증된 적이 없던 여러 building blocks를 적용해봄
> - Concatenated ReLU(C.ReLU), Inception, HyperNet's multi-scale representation

## 접근법
- 먼저, ReLU activations 대신 C.ReLU를 사용함
> - C.ReLU는 네트워크의 초기 feature maps에서 주로 features 간 쌍을 이루는 activation pattern이 나타난다는 점에서 고안되었음
> - 따라서, C.ReLU는 출력 channel 수를 줄이고, 출력 feature maps에 negation을 취한(-1을 곱한) 결과를 기존 출력과 concat하는 방식을 사용함
> - 정확도의 하락 없이 초기 단계에서 두 배의 속도 개선이 가능함
- 다음으로 small/large objects를 cost-effective하게 잘 capture하기 위해 Inception building block을 사용함
> - Large-sized object를 잘 포착하기 위해서는 충분히 큰 receptive field가 필요
> - Small-sized object를 잘 포착하기 위해서는 충분히 작은 receptive field가 필요
> - Inception module은 convolutional block을 여러 개의 convolutional paths로 분할하여 연산하는 특징이 있음 (cardinality)
> - 이때 3x3 convolution이 stack되어 있는 path에서는 큰 receptive field로 large-sized objects의 특징을 잘 포착할 수 있음
> - 반면 소수의 1x1 혹은 3x3 convolution이 있는 path에서는 receptive field가 작게 유지되며 small-sized objects의 특징을 잘 포착할 수 있음
- 또한, ION, HyperNet과 마찬가지로 multi-scale representation을 사용함
> - 최종 layer의 output과 일부 intermediate layers의 outputs를 up-/down-sampling하여 concat한 뒤 최종적으로 사용함

## 실험결과
- PASCAL VOC 2007/2012 데이터셋에서 R-FCN, Faster R-CNN보다 훨씬 빠른 속도와 훨씬 적은 연산량을 기록하였으며, 정확도는 거의 유사하거나 더 좋았음

## 의견
- / 