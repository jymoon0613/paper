# And the Bit Goes Down: Revisiting the Quantization of Neural Networks (ICLR, 2020)

[논문링크](https://arxiv.org/abs/1907.05686)

<p align="center">
    <img width="500" alt='fig1' src="../img/stock2019and.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 convolution의 spatial redundancy를 이용하는 quantization method를 제안함
- Knowleged ditillation 기반의 layer-by-layer compression 수행
> - 각 layer의 output activation에 대한 reconstruction error 최적화

## 접근법
- Product quantization(PQ)을 baseline으로 사용함
> - 주어진 weight matrix의 각 column을 subvectors로 나눔
> - 나누어진 subvectors에 K-means clustering을 적용하고, 각 subvector를 특정 cluster의 centroid로 매핑하여 quantize함
> - Training objective는 기존 weights와 quantized weights의 거리를 최소화하는 것 (K-means를 통해 효과적으로 최적화 가능)
- 본 논문에서는 quantize된 weights의 reconstruction error를 최소화하는 것이 아닌 quantized weights로 계산된 output activation의 reconstruction error를 최소화하는 방법을 사용
> - Weights를 preserve하는 것이 항상 output을 preserve하는 것을 보장하지 않음
> - Weighted K-means 알고리즘을 통해 PQ와 유사하게 최적화 가능
- Fully-connected layer 외에도, convolution의 4D matrix를 vector form으로 변환시켜 convolutional filters에도 적용함
> - Convolutional operation의 spatial redundancy를 고려한 reshaping 및 quantization 수행
- Distillation을 통해 cluster assingment를 fine-tuning 함

## 실험결과
- 다른 SOTA compression 기법들에 비해 좋은 성능 달성함

## 의견
- /