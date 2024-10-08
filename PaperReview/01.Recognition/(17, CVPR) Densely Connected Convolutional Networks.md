# Densely Connected Convolutional Networks (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Huang_Densely_Connected_Convolutional_CVPR_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/huang2017densely.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 컴퓨터 하드웨어와 네트워크 구조의 발전은 인식 성능 향상을 위한 매우 깊은 네트워크 구조 설계를 가능하게 했음
- 하지만 네트워크가 깊어짐에 따라 정보 혹은 gradient가 깊은 네트워크를 통과하면서 점점 사라져버리는 문제가 있었음
- 이러한 문제에 대한 여러 해결방안이 제안되었으며, 그 공통점은 앞 레이어와 뒤 레이어 간의 정보 흐름을 원활하게 하는 shortcut path를 추가해주는 것임
- 본 논문에서는 이러한 insight에 기반하여 네트워크의 각 레이어 간의 정보 흐름을 최대화하기 위해 모든 레이어를 shortcut path를 통해 연결하는 간단한 connectivity pattern을 제안함 (L-layer 네트워크의 경우 L(L+1)/2 만큼의 연결이 존재)
- 이를 통해 네트워크의 한 레이어는 이전의 모든 레이어의 output을 입력으로 받아 처리함 
- 이때 addition을 통해 정보를 결합하는 ResNet과 달리 concat을 통해 결합함
- 이러한 dense connectivity pattern에 기반한 네트워크를 Dense Convolutional Network (DenseNet)으로 칭함

## 접근법
- 네트워크의 각 레이어는 모든 이전 레이어의 출력을 입력으로 받음
- 레이어로 들어온 여러 입력(feature map)을 concat하여 처리함
- 이때 입력으로 받은 feature map의 사이즈가 다르면 concat이 불가능함
- 따라서, {BN, ReLU, 3x3 conv}로 이루어진 layer를 stack하고 dense connectivity를 적용시킨 dense block을 정의하여 네트워크를 dense block 단위로 구성하였으며, block 내에서 resolution을 줄이지 않고 각 block 사이에 downsampling을 수행하는 transition layer를 설계하여 적용
- Transition layer는 1x1 conv와 2x2average pooling으로 구성됨
- DenseNet은 ResNet과 달리 여러 입력을 concat하여 처리하기 때문에 output channel이 자연스럽게 커짐
- 따라서, 한 레이어의 output channel 크기를 growth rate k로 제한함 
> - 즉, $l$번째 레이어의 입력 feature map은 $k_0+k*(l-1)$ 개의 channel 크기를 가짐 ($k_0$은 해당 layer가 속한 dense block의 입력 채널 수) 
- 이때 k=12와 같이 작은 k값을 설정해도 성능이 좋았는데, 이는 (1) 각 레이어가 이전 layer로부터 축적된 네트워크의 collective knowledge를 쉽게 참조할 수 있다는 점과, (2) growth rate는 각 레이어가 네트워크의 global한 정보량에 얼마만큼 기여할 것이가를 결정하는 파라미터이고 이러한 네트워크의 global한 학습 정보는 전체 네트워크에서 쉽게 접근 가능하다는 점에서 기인함
- 또한, {BN, ReLU, 3x3 conv}로 이루어진 기본 layer에 {BN, ReLU, 1x1 conv}을 추가한 bottleneck layer {BN, ReLU, 1x1 conv, BN, ReLU, 3x3 conv}을 사용할 경우 성능이 향상되었으며 이를 DesNet-B라고 칭함 (이때 1x1 conv의 output channel 수는 4k이며 3x3 conv의 output channel 수는 k)
- 하나의 Dense block의 output channel 수 $m$에 대해 transition layer는 output channel 수를 $\theta{m}$으로 줄임
- 이때 $\theta$가 1이면 channel 수는 변하지 않으며 $\theta<1$일 때의 DenseNet을 DenseNet-C라고 칭함 (논문에서는 $\theta=0.5$ 사용)
- 만약 bottleneck layer를 사용하고 $\theta<1$이면 DenseNet-BC라고 칭함

## 실험결과
- DenseNet-BC는 당시의 SOTA 성능을 달성
- Growth rate가 커지고 depth가 깊어질 때 네트워크의 성능이 일관적으로 좋아짐 (오버피팅이 발생하지 않으며 최적화의 어려움이 없었음)
- 또한, bottleneck design의 사용과 transition layer는 파라미터 효율성을 증가시켜줌 (네트워크 깊이에 비해 적은 파라미터 수)
- Concat을 통해 모든 이전 레이어의 feature map을 참조하는 방식은 feature reuse를 장려하고 더 compact한 모델을 학습시킬 수 있음
- 정리하자면, dense connection은 다음과 같은 4가지 장점이 있음
> - Gradient를 다양한 경로를 통해서 전달할 수 있기 때문에 vanishing gradient 문제 감소
> - 앞단에서 만들어진 feature를 그대로 뒤로 전달을 해서 concatenation 하는 방법을 사용하므로 feature를 계속에서 끝단 까지 전달하는 feature propagation 강화
> - low-level feature 부터 high-level feature 까지 concatenation을 통해 사용할 수 있도록 하기 떄문에 feature의 재사용성을 강화하고 성능 향상
> - Growth rate를 통하여 channel의 증감량을 조절할 수 있으며 growth rate를 작게 잡으면 feature를 계속 channel 방향으로 쌓아가더라도 늘어나는 파라미터의 갯수는 많지 않으므로 파라미터 효율성이 높음

## 의견
- /