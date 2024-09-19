# CLIP-Q: Deep Network Compression Learning by In-Parallel Pruning-Quantization (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Tung_CLIP-Q_Deep_Network_CVPR_2018_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/tung2018clip.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Network compression 기술은 두 가지 접근법으로 나눌 수 있음:
> - 네트워크의 test time에서의 inference를 가속화하는 방법
> - 네트워크의 training을 가속화하는 방법
- 그중 첫번째 접근법은 높은 정확도를 위해 많은 수의 weights를 지닌 네트워크가 필요하지만, 네트워크의 학습이 끝난 이후에는 보통 높은 수준의 parameter redundancy가 존재한다는 것에 집중
> - Network pruning: 네트워크 weights 중 덜 중요한 weights를 일부 제거함
> - Weight Quantization: 네트워크 weights를 discrete values가 되도록 제약을 두어서 weights가 더 적은 bits로 표현될 수 있도록 함
- 기존 방법들은 pruning과 quantization을 분리하여 수행하지만, 이 경우 pruning과 quantization의 상호보완적인 특성을 누릴 수 없음
- 본 논문에서는 pruning과 quantization을 하나의 프레임워크로 통합하는 compression learning by in-parallel pruning-quantization(CLIP-Q)를 제안함

## 접근법
- CLIP-Q의 궁극적 목적은 pre-trained 네트워크를 경량화하는 것
> - 불필요한 weights를 제거 (pruning)
> - 남아 있는 weights의 bits 수를 줄임 (quantization)
- 네트워크의 fine-tuning을 진행하면서, 각 layer에서는 3단계로 구성된 pruning-quantization 연산을 수행함
> - Clipping: 각 layer에서 정해진 비율 $p$ 만큼의 positive weights이 $c^+$ 이하, $p$ 만큼의 negative weights이 $c^-$이상이 되도록 함. $c^+$, $c^-$ 사이에 있는 weights는 다음 forward pass에서 0으로 설정됨. 매 iteration마다 update되는 weights에 clipping을 다시 적용하므로 결과는 매번 바뀜.
> - Partitioning: Clip되지 않은 weights를 quantization intervals로 구분함. Uniform하게 partitioning을 수행
> - Quantizing: Partitioning된 quantization intervals에 따라 quantization 진행. 각 quantization interval의 평균 weight를 계산하여 해당 interval에 속하는 weights의 값을 대체. 위의 과정과 마찬가지로 매 iteration마다 quantize되는 weights, quantization values가 변할 수 있음
- Pruning-quantization 결과는 매 iteration마다 바뀌는 특성이 있으며, 이를 위해 full network weights도 따로 저장하면서 업데이트함
> - Quantized weights만 사용하는 compressed network로 loss 계산, loss에 의한 update는 full network weights에 적용
- 학습이 끝난 뒤에 full network weights는 제거되며, quantized weights만 sparse encoding scheme에 따라 저장됨

## 실험결과
- AlexNet, GoogLeNet, ResNet에 CLIP-Q 적용
> - 각각을 51배, 10배, 15배 compress하면서 거의 동일한 정확도를 유지했음

## 의견
- /