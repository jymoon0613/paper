# Designing Network Design Spaces (CVPR, 2020)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Radosavovic_Designing_Network_Design_Spaces_CVPR_2020_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/radosavovic2020designing.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Deep CNN 구조는 visual recognition tasks의 발전을 이끌었으며, 다양한 특징을 지닌 네트워크 아키텍처들이 제안되어 왔음
- 하지만 이렇게 연구자가 직접 네트워크를 설계하는 경우(manual design) 최적의 네트워크 구조를 도출하는 것이 어려울 수 있으며, 특히 선택해야 하는 옵션의 수가 많을 때는 더 어려워짐
- 이러한 문제를 해결하기 위한 방법으로는 nueral architecture search (NAS) 방법을 사용하는 것이 있으며, 이는 네트워크의 모든 가능한 구조가 포함된 fixed search space를 설정하고 그 안에서 최적의 모델을 자동으로 도출하는 방법임
- 하지만 NAS를 사용해서 얻은 최적의 모델은 특정 task, 특정 하드웨어 환경, 특정 데이터셋에만 최적일 수 있음 (single network instance)
- 즉, 도출된 모델은 네트워크의 구조적 원리에 대한 이해에 기반하여 도출된 것이 아니기 때문에 도출 근거가 명확하지 않고 interpretable하지 않을 수 있으며, 또한 general한 환경에서의 사용을 위한 네트워크를 보장하지 못한다는 한계가 있음
- 오히려 위와 같은 interpretable하고 generalizable한 네트워크 구조는 manual한 디자인을 통해 달성할 수 있음
- 따라서, 본 논문에서는 manual design과 NAS의 장점을 결합한 새로운 네트워크 디자인 패러다임을 제시함
- 제안하는 방법은 특정 network instance를 찾는 것이 아니라 네트워크 구조 서치가 이루어지는 design space 자체를 디자인함
- 이러한 design space design을 통해 네트워크의 구조적 원리에 기반한 interpretable한 insigt를 얻을 수 있으며 간단하고, 성능이 좋고, generalizable한 모델을 도출할 수 있음
- 핵심 아이디어는 초기의 아무런 제약이 없는 design space로부터 지속적으로 refine을 수행하여 최적의 design space를 도출하는 것
- 또한, 본 연구를 통해 간단한 regular networks들이 속해 있는 desing space를 도출할 수 있었으며, 해당 desing space에 속하는 네트워크인 RegNet은 이전의 SOTA 모델들보다 인식 성능이 좋고 빨랐음

## 접근법
### (1) Overview
- 먼저 desing space를 평가하기 위한 tool로는 desing space로부터 샘플링된 모델 집합의 error distribution을 비교하는 방법을 사용 (추출된 모델 집합의 전체 모델 중 error가 특정값보다 작은 모델의 수)
- Design space design을 수행하는 과정은 (1) 위에서 설정한 tool로 desing space의 quality를 평가하고, (2) 평가 결과 시각화를 통해 insight를 얻고, (3) insight를 바탕으로 design space를 reifne함 (manual + NAS)

### (2) AnyNet Design Space
- Initial design space는 AnyNet이라고 칭함
- AnyNet에서는 block의 수(depth), block의 width(채널의 수), bottleneck block의 비율 등 네트워크의 다양한 구조에 집중함
- AnyNet을 구성하는 네트워크의 기본적 디자인은 (1) 입력 처리와 간단한 연산을 포함하는 stem, (2) 대부분의 연산을 처리하는 body, (3) 최종 분류를 수행하는 head로 구성됨
- Stem과 head는 최대한 간결하게 유지하고 네트워크의 계산량과 성능에 영향을 크게 미치는 body의 구조에 집중함
- Body는 점차 feature map의 resolution이 감소하는 4개의 스테이지로 구성되며, 각 스테이지는 같은 모양의 block이 여러개 축적된 형태로 구성
- 각 스테이지마다 depth, width를 포함하는 block parameters를 변경할 수 있음
- 대부분의 실험은 일반적인 group convolution이 적용된 residual bottleneck block이 사용되었으며(x block), 해당 block을 사용하는 AnyNet desing space를 AnyNetX로 칭함
- AnyNetX는 16 자유도를 지님: 4stage x 4params(depth, width, bottleneck ratio, group width) (input resolution은 224x224로 고정)
- AnyNetX design space에서 500개의 모델을 추출하고 10epoch를 훈련시켜 설정한 tool로 design space를 평가
- 이로부터 desing space를 분석하여 insight를 도출하고, design space를 지속적으로 refine함
> - AnyNetX_B: 모든 stage에서 bottleneck ratio를 단 하나의 수로 고정하였을 때 성능의 차이가 없었으므로 bottleneck ratio를 고정하여 자유도를 감소시킴
> - AnyNetX_C: 모든 stage에서 group width를 단 하나의 수로 고정하였을 때 성능의 차이가 없었으므로 group width도 고정하여 자유도를 감소시킴
> - AnyNetX_D: 네트워크의 뒤로 갈수록 block의 width를 키우는 제약을 주었을 때 네트워크의 성능이 개선됨
> - AnyNetX_E: 네트워크 뒤로 갈수록 block의 depth를 키우는 제약을 주었을 때 네트워크의 성능이 개선됨
- 위 refine 과정을 통해 자유도가 줄어들고 성능이 더 높은 모델을 추출할 수 있는 desing space를 도출할 수 있었음

### (3) RegNet Design Space
- 도출한 AnyNetX_E에서 더 깊은 insight를 얻기 위해 20개의 best model의 성능을 평가하고 분석함
- 네트워크를 결정하는 변수들을 6개의 파라미터로 정리하여 desing space를 추가로 refine함
- 결과로 얻은 이러한 간단하고 regular한 design space를 RegNet이라고 칭함
- 이때 RegNet은 RegNetX이며, RegNetX에 squeeze and excitation(SE) 연산을 추가한 desing space를 RegNetY로 칭함

## 실험결과
- 높은 연산량의 모델일수록 3번째 스테이지의 block 수가 가장 많았으며, 마지막 stage의 경우 block수가 적었음 (ResNet의 디자인 구조와 유사)
- 또한, group width는 더 큰 모델에 따라 그 값이 증가했고 depth는 saturation됨 (어느정도 증가하다가 유지)
- Mobile device에 사용할 목적으로 가벼운 네트워크를 설계할 경우 RegNetX와 RegNetY는 다른 가벼운 모델들(MobileNet, ShuffleNet)보다 성능이 좋거나 경쟁력 있는 수준이었으며, RegNet은 아무 부가 기능도 들어가 있지 않은 basic한 모델이므로 추가적인 성능 개선의 여지가 있음
- 가벼운 모델에서는 EfficientNet이 성능이 더 좋았고 높아질수록 RegNetY와 RegNetX 모두 EfficientNet보다 좋아짐
- RegNet은 EfficientNet보다 5배 정도 빠르고 더 적은 에러를 달성

## 의견
- /