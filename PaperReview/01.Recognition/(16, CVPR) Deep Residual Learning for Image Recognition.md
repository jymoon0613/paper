# Deep Residual Learning for Image Recognition (CVPR, 2016)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/he2016deep.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Deep convolutional neural networks (Deep CNN)은 이미지 분류에서 꾸준히 좋은 성능을 내왔음
- Deep CNN은 low/mid/high level 피처를 multi-layer 구조를 통해 통합하며 피처의 level은 모델의 stacked layers 수 (depth)에 의해 풍부해짐
- Deep CNN의 구성에 있어서 depth는 매우 중요하며 최근의 많은 연구에서는 성능 향상을 위한 매우 깊은 네트워크 구조를 제안하고 있음
- 기존에는 vanishing/exploding gradient 문제로 인해 무작정 깊은 네트워크를 설계하는 것이 문제가 되었지만 normalized initialization, normalization layer의 도입은 이러한 깊은 네트워크가 수렴할 수 있도록 해줌 
- 하지만, 깊은 네트워크의 수렴이 가능하다고 할지라도, degradation 문제가 발생함: 네트워크의 깊이가 깊어질수록 성능은 일정 수준으로 점점 포화되고 오히려 성능이 점점 감소하기 시작함
- 이러한 degradation은 training 성능 평가에서도 발견된다는 점에서 overfitting 문제와는 다름
- '깊은 네트워크가 얕은 네트워크보다 항상 성능이 좋거나 같아야 한다'는 가정을 충족시킬 수 있는 하나의 솔루션은 얕은 네트워크와 해당 네트워크에 몇 개의 레이어를 추가한 깊은 네트워크를 두고, 깊은 네트워크는 얕은 네트워크의 학습된 파라미터를 사용하되 추가된 레이어는 입력을 그대로 출력에 매핑하는 identity mapping을 수행하도록 설계하는 것임
- 이러한 간단한 솔루션은 최소한 깊은 네트워크가 얕은 네트워크보다 성능이 나쁘지는 않은 결과를 보장함
- 하지만, 저자들은 현재 이러한 간단한 솔루션보다 더 나은 성능을 보이는, 즉, 깊은 네트워크가 얕은 네트워크보다 확실히 성능이 더 좋도록 하는 솔루션은 찾을 수 없다고 주장함
- 따라서, 본 논문에서는 deep residual learning framework을 통해 degradation 문제를 해결하고자 하며, 이러한 residual learning이 적용된 Deep CNN인 ResNet을 제안함
- 본 논문의 기본적 가설은 기존의 방식처럼 알 수 없는 출력값에 대한 매핑(unreferenced mapping)을 최적화하는 것보다 잔차 매핑(residual mapping)을 최적화하는 것이 더 쉽다는 것
- 즉, 기존의 레이어는 입력 x에 대해 출력 H(x)를 매핑하도록 훈련되었다면, residual mapping은 입력 x에 대해 잔차 F(x) = H(x) - x를 매핑하도록 훈련됨
- 이는 다시 말해 모델이 입력 x에 대해 H(x) = F(x) + x를 매핑하도록 훈련된다는 것을 의미함
- 이러한 모델 연산은 skip connection으로 구현할 수 있음 (입력 x에 대해 레이어 연산을 통해 F(x)를 추출하고 입력 x를 다시 더해줌)

## 접근법
- Multiple non-linear layers가 어떠한 복잡한 function이든 점진적으로 근사할 수 있으므로, 같은 이유로 residual functions을 근사할 수 있음
- 즉, multiple non-linear layers가 점진적으로 출력 H(x)를 근사할 수 있듯이, 잔차 H(x) - x도 근사할 수 있음
- 이는 결국 레이어가 출력 H(x) = F(x) + x를 출력하도록 최적화된다는 것을 의미함
- 앞서 degradation이 발생하지 않도록 하는 가장 간단한 솔루션은 학습된 shallow network 위에 identity mapping을 수행하는 레이어를 쌓는 것이라 했음
- 간단한 솔루션이 존재함에도 불구하고 degradation이 발생하는 이유는 일반적인 학습 구조 (입력 x로부터 출력 H(x)를 매핑)에서 모델은 multiple nonlinear layers가 identity mapping을 수행하도록 학습시키는 데 문제를 겪고 있다는 것을 의미함
- 이는 residual learning을 사용한다면 쉽게 해결될 수 있는데, 레이어의 출력은 H(x) = F(x) + x이므로 identity mapping을 위해서는 단순히 multiple nonlinear layers의 가중치를 0으로 두면 됨 (H(x) = 0 + x)
- 물론 identity mapping이 optimal한 솔루션인 경우는 드물지만, 실제 솔루션이 identity mapping에 가까운 경우나 혹은 문제의 해결 방법 자체에 대한 난이도는 residual learning이 훨씬 쉬움
- 실제 구현에서 입력 x의 처리 결과 F(x)에 원본 입력 x가 element-wise addition으로 더해짐
- 만약 dimension이 안맞으면 입력에 별도의 linear projection을 적용하여 dimension을 맞춰준 뒤 F(x)에 더함 
- Residual learning은 적용되는 block에 최소한 2개 이상의 non-linear layer로 구성되어 있을 때 효과가 있었음 (단일 layer로 구성된 block의 경우 성능 이점 발견 못함)

## 실험결과
- 같은 depth를 지니는 네트워크에 대해 plain한 경우와 달리 residual한 경우 depth가 깊어질수록 성능 증가가 나타났음
- 즉, residual learning을 사용할 때 degradation 문제가 해결됨
- ResNet-18, ResNet-34와 같은 shallow한 구조에는 2개의 3x3 convolutional layer로 이루어진 basic block이 사용되며, ResNet-50, ResNet-101, ResNet-152와 같은 더 깊은 구조에는 1x1, 3x3, 1x1의 3개의 convolutional layer로 이루어진 bottleneck block이 사용됨
- ImageNet challenge의 SOTA 성능 달성
- CIFAR-10에서도 성능 검증했으며, 모델 구조는 ImageNet 버전과 살짝 다름
- CIFAR-10에서도 residual learning이 degradation을 방지하는 데 효과가 있었음
- 각 레이어에서의 reponse의 standard deviations를 계산하여 분석함 (response = batch normalization을 거치고 ReLU가 적용되기 전의 convolutional layer의 출력)
- 네트워크의 깊이가 매우 깊어지는 경우 기존의 결과와 달리 깊은 모델의 성능이 얕은 모델보다 안좋아짐 (110-layer vs. 1202-layer)
- 하지만 이는 네트워크가 데이터의 양에 비해 과하게 깊어져서 오버피팅이 발생한 것이며, 이 때는 dropout과 같은 regularization 방안이 필요해보임

## 의견
- 솔루션 도출 방식이 매우 논리적임
- 이론적 설명과 실험 모두 매우 뛰어남