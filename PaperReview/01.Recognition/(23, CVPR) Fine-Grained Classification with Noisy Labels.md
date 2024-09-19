# Fine-Grained Classification with Noisy Labels (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2303.02404)

<p align="center">
    <img width="600" alt='fig1' src="../img/wei2023fine.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Large-scaled 데이터 구성 작업에서 label noise는 불가피하게 발생할 수 있으며, 이는 모델의 generalization performance의 악화로 이어짐
- Learning with noise labels(LNL)은 주어진 label-corrupted training set에서도 준수한 generalization performance를 달성하는 것을 목표로 함
> - LNL-FG는 annotators가 inter-class의 subtle difference 때문에 label 할당을 실수하기 더 쉽기 때문에 LNL의 보다 현실적인 시나리오임
> - 또한, 기존 LNL(generic classification)에서 좋은 성능을 보여주었지만 LNL-FG에서는 효과가 거의 없는 경우가 많았고 심지어는 generalization performance가 감소하기도 함
> - 이는 LNL-FG의 큰 inter-class ambiguity로 인해 노이즈 샘플과 decision boundary 사이의 거리가 generic classification의 경우보다 가까워지시 때문이며, 이때 모델은 노이즈 레이블에 심하게 오버피팅될 수 있음
- 따라서 discriminative한 features는 레이블 노이즈에 오버피팅되지 않으면서 fine-grained objects 학습에 용이해야 함
- 이러한 향상된 표현을 학습하기 위해 본 논문에서는 contrastive learning(CL)기법 활용을 고려하였으며, 특히 레이블 정보를 사용하여 representation learning을 더 향상시키는 supervised contrastive learning(SCL)을 사용하고자 함
- 하지만 SCL은 noise-tolerated한 매커니즘이 없기 때문에 레이블 노이즈 시나리오에 바로 적용할 수 없음
- 따라서, 본 논문에서는 SCL의 noise-sensitivity를 해소하기 위해 noise-tolerated contrastive loss와 stochastic module을 포함하는 stochastic noise-tolerated supervised contrastive learning(SNSCL)을 제안함

## 접근법
- Noised-tolerated supervised contrastive learning method(NTSCL)은 weight-aware mechanism과 SCL를 결합하여 정의됨
- Weight-aware mechanism은 각 training sample의 reliability score를 측정하여 각 샘플에 상응하는 weight를 생성함
> - Two-component GMM을 사용하여 reliability score 계산
> - Reliability score가 특정 threshold를 넘은면 해당 샘플의 weight는 1이 되며, 아닌 경우 reliability score가 해당 샘플의 weight가 됨
> - Weight와 reliability score는 매 epoch마다 업데이트됨
- 이러한 weight-aware mechanism을 바탕으로 SCL의 두 가지 noise-sensitive issues를 해결함
- 먼저, sample weight를 통해 ground-truth 레이블과 모델 예측 레이블의 weighted sum으로 계산된 soft label을 생성함
- 이때 레이블 대체 과정을 안정적으로 만들기 위해 moving-average 과정을 추가로 사용함
- 이것은 label noise로 인해 SCL의 positive, negative anchors가 misguide되는 현상을 해소함
- 또한, SCL의 momentum queue의 noise-tolerance를 증진시키기 위해 weighted update strategy를 사용함
- 이는 momentum queue에 unreliable한 샘플이 들어가는 것을 막음
- 또한, SCL 사용 시 기존 CL처럼 사전에 정의된 augmentation을 사용하는 것이 아니라 stochastic feature embedding 전략을 사용함
> - Backbone의 output embedding에 대해 gaussian probability distribution을 정의하고, 해당 분포로부터 샘플링된 representation을 augmented된 sample로 사용함

## 실험결과
- 기존 FG 데이터셋에 임의의 label noise를 준 뒤 성능을 평가함
- 기존 LNL 방법에 SNSCL을 추가함(plug-in)
- 기존 LNL 방법들보다 FG에서의 성능이 좋았음
- 또한 웹사이트에서 수집한 real-world 데이터셋에서의 성능도 평가함: Clothing-1M, Food-101N

## 의견
- /