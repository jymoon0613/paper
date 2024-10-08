# Network In Network (ICLR, 2014)

[논문링크](https://arxiv.org/abs/1312.4400)

<p align="center">
    <img width="600" alt='fig1' src="../img/lin2014networknetwork.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Convolution layers는 input을 처리하기 위해 linear filters와 non-linear activation function을 사용함
- 즉, convolution filter는 주어진 local part(receptive field)에 대한 generalized linear model(GLM)이라고 볼 수 있음
> - 하지만 GLM의 추상화 수준은 낮음
- GLM을 보다 강력한 non-linear function approximator로 교체한다면 추상화 능력을 향상시킬 수 있음
> - GLM은 데이터의 latent space가 linearly separable한 경우 잘 작동하지만, 대부분의 데이터는 latent space가 non-linear한 manifold 형태를 보이기 때문에 이를 모델링할 수 있는 매우 non-linear한 function이 필요함
- 따라서, 본 논문에서는 GLM을 general한 non-linear function approximator인 micro network로 대체하며, micro network 구조로는 multi-layer perceptron(MLP)을 사용함 (mlpconv layer)
> - 기존의 conv. layer와 mlpconv layer 모두 local receptive field를 output feature vector로 mapping함
> - 단, mlpconv는 복수의 fully connected layers(FC layers) 및 non-linear activation function으로 구성된 MLP로 mapping을 수행함
- 이러한 mlpconv layers를 여러 층 쌓아 네트워크 구조를 설계함 (Network in Network, NIN)
- 또한, 분류를 수행하기 위해 기존 CNN 구조에서 사용하였던 FC layers 대신에, global average pooling(GAP) layer를 사용함

## 접근법
- Classic한 convolution layers는 linear convolutional filters와 non-linear activation functions로 이어지는 연산을 통해 feature maps를 생성함
- CNN에서 높은 층의 layer는 original input의 더 넓은 영역을 매핑하고 이전 layer를 combining하여 더 추상적인 concept을 만듦
- 따라서 이전 layer를 combining하기 전에 각 local patch에 대해 더 나은 추상화 과정을 거치면 성능이 더 좋아질 수 있음
> - NIN의 주요 아이디어
- 기존 convolution layer에서 input-output mapping을 담당하던 linear convolutional filter를 MLP로 대체함(mlpconv)
- 또한, 분류 과정에서 기존 CNN의 FC layers를 사용하지 않고 GAP를 사용함
> - GAP는 FC layer를 거치지 않고 카테고리를 나눴기 때문에 feature map과 category간에 관련성이 높아 결과를 설명하기 쉬움
> - 최적화해야하는 parameter가 없으므로 overfitting을 피할 수 있음
> - Spatial information에 대해 더하고 평균을 내는 것이므로 입력 데이터의 spatial translation에 robust함
- NIN은 mlpconv layers를 여러 개 쌓아서 구성한 네트워크
> - 각 mlpconv layer의 내부에는 3-layer MLP가 micro network로서 존재함

## 실험결과
- Mlpconv layer는 local patch를 더 잘 모델링함
- GAP는 overfitting을 방지함

## 의견
- /