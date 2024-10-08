# Aggregated Residual Transformations for Deep Neural Networks (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Xie_Aggregated_Residual_Transformations_CVPR_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/xie2017aggregated.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- VGG-Net와 ResNet은 같은 shape의 block을 여러 개 축적하여 깊은 네트워크를 설계함
- 이러한 네트워크의 깊이(depth)에 대한 고려는 recognition 성능 향상에 중요할뿐만 아니라, 네트워크가 특정 데이터셋(or task)에 과적합되지 않도록 하여 다른 vision tasks 또는 음성 및 언어를 사용하는 non-vision tasks에서의 견고성 지니고 쉽게 일반화될 수 있도록 함
- 또한 Inception 계열의 모델(Inception module)은 block 구조의 신중한 설계를 통해 복잡도를 줄이면서 경쟁력있는 성능을 달성할 수 있다는 것을 증명함
- Inception module은 split-transform-merge 전략을 사용하는데, 이는 하나의 convolution 연산(ex. 5x5 convolution)을 적용하는 대신, 입력을 1x1 convolution을 통해 몇개의 lower-dimensional embeddings로 나누고(split), 일련의 convolution 연산(ex. 3x3 and 5x5 convolution)을 적용하여(transform), 결과를 취합함(merge)
- 즉, 입력을 여러 경로로 나누어 연산(변환)한 뒤 종합하는 방식
- 하지만, 이러한 Inception의 방법은 어떤 필터 크기의 convolution 연산을 조합할 것인지, 몇개를 사용할 것인지 등 고려해야하는 하이퍼파라미터의 수가 많으며 각 path마다, tasks마다 최적 설정값이 모호할 수 있음
- 따라서, Inception module의 장점은 취하면서 하이퍼파라미터에 대한 고민을 줄이기 위해 VGG-Net/ResNet처럼 기본 모듈을 잘 정의하고 같은 구조를 연속적으로 쌓아올리는 방법을 적용할 수 있음
- 이를 위해, 본 논문에서는 Inception module과 마찬가지로 split-transform-merge 전략을 사용하는 block을 사용하되, Inception module처럼 다양한 필터 크기, 수의 transformation이 조합되는 것이 아닌 모두 같은 transformation이 적용될 수 있도록 함 (ResNeXt)
- 또한, 본 연구에서 제안하는 방법(ResNeXt)은 cardinality(transformation을 수행하는 그룹의 수, 즉, Inception block에서의 path 수)가 네트워크의 깊이나 넓이보다 성능 향상에 주요함을 보여줌

## 접근법
- ResNeXt는 ResNet과 마찬가지로 같은 shape의 residual block을 쌓아올림
- ResNeXt는 ResNet 및 VGG-Net의 concept와 마찬가지로 (i) 같은 파라미터를 공유하는 block 단위의 구성과 (ii) 블럭의 spatial map 크기가 반으로 줄어들면 (feature map 사이즈가 반으로 감소) block의 width를 2배 증가시켜 (channel의 차원을 2배로 증가 = filter의 수를 2배 증가) block별 연산량이 비슷하게 유지되도록 함
- ResNeXt는 ResNet의 residual block을 c (cardinality)개의 연산으로 나누어 처리하도록 설계함
- 이때 c개의 연산은 모두 같은 구조(topology)를 지님
- c개의 연산 결과는 종합되며 입력에 대해 skip-connection으로 연결되어 최종적으로 산출됨

## 실험결과
- 비슷한 크기의 ResNet 모델보다 좋은 성능을 보임
- Cardinality가 증가할수록 성능이 좋아짐
- 단순히 네트워크의 용량을 증가시키는 방법(depth나 width 증가)보다 cardinality를 증가시키는 방법이 성능 향상에 더욱 중요함

## 의견
- /