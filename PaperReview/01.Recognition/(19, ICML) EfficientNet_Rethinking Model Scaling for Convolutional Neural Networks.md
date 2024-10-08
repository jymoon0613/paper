# EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks (ICML, 2019)

[논문 링크](http://proceedings.mlr.press/v97/tan19a.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/tan2019efficientnet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 모델의 크기를 키움으로써 성능을 높이는 방향의 연구가 많이 제안되었음
- 모델의 크기를 키우는 방법으로는 (1) 모델의 depth를 증가시키거나 (layer 추가), (2) 모델의 width를 증가시키거나 (filter 개수 추가=channel dimmension 증가), (3) 입력 이미지의 resolution을 키우는 방법이 있음
- 이러한 3가지 차원 중 하나에만 집중하였던 이전의 연구들과 달리, 본 연구에서는 3가지 차원의 최적의 조합을 찾고자 함
- 특히, 본 논문에서는 3가지 차원의 최적 조합을 효과적으로 찾을 수 있도록 compound scaling 방법을 제안
- 또한, 이러한 model scaling이 효과적으로 적용될 수 있는 새로운 baseline인 EfficientNet을 제안함

## 접근법
- 본 논문이 목표로 하는 것은 resource에 대한 constraints를 충족시키면서 최대의 model accuracy를 달성하는 것
- 이는 이러한 objective를 충족시키는 최적의 depth, width, resolution이 서로 깊게 연관되어 있으며 resource constraints의 변화에 따라 값이 변하기 때문
- Depth:
> - 네트워크의 깊이가 증가할수록 모델의 capacity가 커지고 더 복잡한 feature를 잡아낼 수 있지만, vanishing gradient의 문제로 학습시키기가 더 어려워짐
> - 이를 해결하기 위해 Batch Norm, Residual Connection 등의 여러 기법들이 등장
- Width:
> - 각 레이어의 width를 키우면 정확도가 높아지지만 계산량이 제곱에 비례하여 증가
- Resolution:
> - 입력 이미지의 해상도를 키우면 더 세부적인 feature를 학습할 수 있어 정확도가 높아지지만 마찬가지로 계산량이 제곱에 비례해 증가
- 세 가지 차원은 공통적으로 어느 정도 이상 증가하면 모델의 크기가 커짐에 따라 얻는 정확도 증가량이 매우 적어짐
- 더 높은 해상도의 이미지에 대해서 네트워크를 깊게 만들어서 더 넓은 영역에 걸쳐 있는 feature(by larger receptive fields)를 더 잘 잡아낼 수 있도록 하는 것이 유리하며, 더 큰 이미지일수록 세부적인 내용도 많이 담고 있어서, 이를 잘 잡아내기 위해서는 layer의 width를 증가시킬 필요가 있음
- 즉, depth, width, resolution은 밀접하게 연관되어 있으며 이들을 같이 변경하면서 모델을 scaling하는 것이 더 효과적임 
- 이를 위해 compound scaling method를 사용함
- Compound scaling method에서는 depth, width, resolution을 일괄적으로 조정하기 위한 compound coefficient $\phi$를 사용함
- Baseline으로는 MnasNet에 기반하여 설계한 EfficientNet을 사용함
- 우선 $\phi=1$로 고정한 뒤 기본 depth($\alpha$), width($\beta$), resolution($\gamma$)를 찾음 (EfficientNet-B0)
- 이후 $\phi$를 변화시키면서 전체적으로 모델을 scaling함 (EfficientNet-B1 ~ EfficientNet-B7)
- 모델의 depth $d=\alpha^\phi$, width $w=\beta^\phi$, resolution $d=\gamma^\phi$
- 이때 $\alpha*\beta^2*\gamma^2$가 2와 최대한 가깝도록 함으로써 설정한  $\phi$에 대해 모델의 총 연산량은 $2^\phi$가 되도록 함 (EfficientNet-B0의 경우 $\alpha=1.2, \beta=1.1, \gamma=1.15$)

## 실험결과
- 최적의 모델 scaling을 적용함으로써 더 적은 연산량 및 더 작은 파라미터 사이즈로도 더 좋은 정확도 달성
- Depth, width, resolution은 서로 긴밀히 연관되어 있으며 이들을 같이 키우는 것이 자원을 더 효율적으로 쓰는 방법임

## 의견
- 한정된 자원을 갖고 있는 상황에서 depth, width, resolution을 어떻게 적절히 조절하여 모델의 크기와 연산량을 줄이면서도 성능은 높일 수 있는지에 대한 연구를 훌륭하게 수행