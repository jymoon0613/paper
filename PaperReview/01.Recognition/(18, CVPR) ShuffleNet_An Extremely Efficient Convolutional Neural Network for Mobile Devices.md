# ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices (CVPR, 2018)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhang_ShuffleNet_An_Extremely_CVPR_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2018shufflenet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Recognition 성능을 높이기 위해 매우 깊고 큰 CNN 구조들이 제안되어 왔음
- 이러한 모델들은 drone, robot, smartphone 등 일반적인 모바일 플랫폼에 적용되기에는 매우 크고 연산량이 많음
- 본 논문에서는 모바일 플랫폼과 같은 매우 제한된 컴퓨팅 환경에서도 경쟁력있는 인식 정확도를 보여주는 모델을 제안함
- 이전의 방법들은 기존의 네트워크 아키텍처를 pruning하거나, compressing하거나, low-bit representing했다면, 본 논문에서는 효과적인 네트워크 아키텍처 자체를 설계하는 것을 목적으로 함 
- Xception, ResNeXt은 1x1 convolution을 사용하여 파라미터 수를 줄일 수 있었지만 결과적으로 1x1 convolution은 모델의 전체 연산량에서 많은 비율을 차지하게 된다는 단점이 존재함
- 본 논문에서는 이러한 1x1 convolution은 큰 연산량을 줄이기 위해 point-wise group convolution 방법을 제안함
- 또한 group convolution으로 인한 부작용을 해결하기 위해 feature channel 간의 정보 공유를 활성화하는 channel shuffling 방법을 제안함
- 이러한 두 가지 요소를 합쳐서 매우 효율적인 네트워크인 ShuffleNet을 구성함

## 접근법
### (1) Channel Shuffle for Group Convolution
- Xception과 ResNeXt은 1x1 convolution(point-wise convolution)을 사용하여 파라미터 수를 줄일수 있었지만 그 연산량에 대해서는 고려하지 않음
- 가벼운 네트워크를 설계해야 하는 환경에서 이렇게 연산량이 큰 1x1 convolution은 모델이 제한된 수의 channel만 사용할 수 있도록 제한하며, 이는 모델의 표현력 약화로 이어짐
- 이를 위해, 1x1 convolution도 group convolution을 적용하는 방법을 제안함
- 각 1x1 convolution이 해당하는 채널 그룹에만 적용되므로 연산량이 줄어듦
- 하지만 group convolution은 group 사이의 교류를 제한함으로써 해당 group들 사이에서만 정보가 흐르도록 함
- 이러한 결과가 축적되면 결국 모델의 representation을 약화 (group별로 독립적인 네트워크 학습하는 것과 같은 작용)
- 따라서, 한 번의 group convolution을 수행하고 다음 group convolution을 수행하기 전 각 그룹의 채널을 서로 섞어줌
- Channel shuffling은 간단한 matrix 연산으로 구현 가능 (reshape -> transpose -> reshape)

### (2) ShuffleNet Unit
- ShuffleNet은 channel shuffle이 적용된 ShuffleNet unit이 stack되어 구성
- ShuffleNet unit은 1x1 group convolution, 3x3 depth-wise convolution, 1x1 group convolution이 순차적으로 이어지는 bottleneck 구조
- 첫 번째 1x1 group convolution 수행 이후 channel shuffle 진행
- 또한 3x3의 average pooling이 적용된 shorcut path도 추가

## 실험결과
- Point-wise group convolution을 통해 제한된 연산량에서 표현력을 향상
- Channel shuffle을 사용했을 때 error가 추가적으로 감소
- MobileNet과 비슷한 complexity를 맞추어 실험했을 때 더 좋은 성능을 보임

## 의견
- /