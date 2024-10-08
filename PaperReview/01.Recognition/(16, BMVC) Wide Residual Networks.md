# Wide Residual Networks (BMVC, 2016)

[논문 링크](https://arxiv.org/abs/1605.07146)

<p align="center">
    <img width="600" alt='fig1' src="../img/zagoruyko2017wideresidualnetworks.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이전까지 CNN은 깊이를 증가시키는 방향을 발전해왔으며, 깊이의 증가에 따라 발생하는 문제들을 해결하기 위한 여러 방법들이 제안되어 왔음
- Deep CNN 모델 중 ResNet은 높은 인식 성능을 보여주었으며, 일반화가 잘되는 경향이 있었고, 수렴 속도도 빨랐음
- ResNet은 skip connection을 통한 identity mapping으로 깊은 네트워크의 원활한 학습을 가능하게 했지만, 이는 다른 측면에서 보면 일부 레이어만 유효한 representation을 학습하며 나머지 레이어는 아무것도 학습하지 않는 문제가 있을 수 있음 (실제로 학습 중 사용되는 레이어를 랜덤하게 선택하여 모든 레이어가 representation을 학습하도록 하는 방법이 제안되기도 했음)
- 따라서, 본 논문에서는 네트워크의 깊이(depth)에 집중하는 대신 네트워크의 넓이(width)에 집중하며, ResNet blocks의 넓이를 증가시키는 것이 깊이를 증가시키는 것보다 좋은 성능 향상을 가져온다는 것을 증명함 (Wide Residual Network, Wide ResNet, WRN)

## 접근법
- 우선 WRN은 기존 ResNet의 bottleneck block은 고려하지 않고 basic block에 대해서만 고려함
- 또한, 선행 연구로부터 conv-BN-ReLU 순으로 연산을 구성하는 것보다 BN-ReLU-conv 순으로 연산을 구성하는 것이 성능이 더 좋다는 것을 확인하고 이를 적용함
- WRN은 기존의 각 conv layer가 갖고 있는 필터 수를 k배하여 width를 확장함 (k=widening factor, k=1이면 ResNet과 동일)
- 또한, basic block의 conv layer 사이에 dropout 추가

## 실험결과
- 거의 모든 경우에 Widening은 일관적으로 성능을 향상시킴
- Depth와 width를 모두 증가시키는 것은 parameter의 수가 너무 높아지거나 혹은 더 강력한 regularization이 요구될 때까지는 성능 향상에 긍정적임
- k=4인 40레이어 깊이의 WRN-40은 ResNet-1001과 거의 유사한 accuracy를 가지지만 8배 더 빠름
- 일반 network와 wide network 모두 dropout을 사용하였을 때 상당한 성능 향상을 관측함

## 의견
- /