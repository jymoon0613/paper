# The Devil is in the Channels: Mutual-Channel Loss for Fine-Grained Image Classification (IEEE T-IP, 2020)

[논문 링크](https://ieeexplore.ieee.org/abstract/document/9005389)

<p align="center">
    <img width="600" alt='fig1' src="../img/chang2020devil.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Visual Classification (FGVC)은 하부 클래스 구분을 위한 특징들이 매우 미세하고 이미지의 작은 부분에 담겨 있으므로 기존의 이미지 분류 문제보다 어려움
- 기존의 접근법으로는 이미지에 manual part annotations을 추가하는 방식이 있지만, 이는 전문지식을 동반한 expensive한 annotation 과정이 필요하다는 점에서 scalable한 방법이 아님
- 따라서, 이러한 discriminative feature localization을 unsupervised하게 하려는 접근 방식들도 제안되었음
- 하지만, annotation이 없다보니 네트워크가 매우 복잡해짐: part detection을 위한 별도의 네트워크 구조가 마련되어 있으며 이를 강화하기 위한 방법이 추가됨
- 본 논문에서도 part annotations 없이 discriminative feature localization을 수행하지만, 복잡한 part detection 네트워크 없이 단일 loss function으로 이를 구현함: Mutual-Channel Loss (MC-loss)
- 따라서, 본 논문에서 제안하는 방식은 별도의 파라미터 추가가 없으며 다른 기존의 네트워크 구조에 쉽게 적용될 수 있음
- MC-loss는 피처맵의 채널에 직접 적용되며, 이를 통해 같은 클래스에 속하는 피처 채널들은 다른 클래스의 피처 채널들과 구분되도록 학습되며 (discriminative), 각 채널은 이미지의 서로 다른 part를 detect하도록 학습됨 (mutually exclusive)

## 접근법
### (1) Overview
- 백본 CNN으로부터 피처맵 추출
- 피처맵의 채널은 전체 c개의 클래스와 각 클래스를 표현하는 채널의 수 zeta로 구성됨 (N = c x zeta)
- 출력 피처맵은 각 클래스에 대해 채널 기준으로 나뉘어짐
- 먼저 전체 피처맵은 기존과 마찬가지로 FC 레이어를 통해 분류에 사용되며 CE loss를 통해 학습됨
- CE loss는 global discriminative features를 찾기 위한 역할임
- 반면 MC-loss는 local discriminative features를 찾기 위한 역할을 하며 최종 loss는 CE와 MC-loss의 합
- MC-loss는 discriminality component와 diversity component의 weighted sum으로 이루어져 있음

### (2) The Discriminality Component
- discriminality component는 서로 다른 클래스에 할당된 피처 채널들이 서로 잘 구분되도록 (discriminative)하는 역할을 함
- 먼저, 각 클래스를 담당하는 채널 그룹에 대해 절반의 채널이 랜덤하게 마스킹됨
- 이후, channel-wise attention이 적용됨
- Channel-wise attention은 strict하게 적용되어 모든 채널이 discriminative한 특징을 지니도록 함 
- Cross-channel max pooling을 통해 채널 정보를 aggregation함
- 마지막으로 global average pooling을 통해 하나의 scalar로 aggregation함
- 각 클래스에 대한 피처 벡터에 적용하면 출력 피처맵은 c 차원의 벡터로 표현가능함
- CE loss를 통해 ground truth label과의 classification error 계산

### (3) The Diversity Component
- 각 피처 채널이 서로 다른 local parts를 detect하도록 분산시키는 역할
- Softmax와 cross-channel max pooling을 적용함
- 클래스를 구성하는 각 피처 채널이 서로 다른 local parts를 detect할수록 diversity measure는 커짐
- diversity measure를 최대화하는 방향으로 학습하기 때문에 diversity loss는 마이너스 부호가 붙음

## 실험결과
- CUB-200-2011, FGVC-Aircraft, Stanford Cars, Flowers-102 데이터셋에서 검증
- 데이터셋마다 클래스별 할당되는 피처 채널의 수 (zeta)는 유동적으로 바뀜
- MC-loss를 사용했을 때 여러 베이스라인 모델들의 성능을 개선
- 각 구성요소, zeta, 다른 loss function과의 비교를 통해 ablation study 수행
- 시각화 결과도 제공

## 의견
- FGVC에서 채널 정보는 확실히 중요함
- Loss를 구성하는 아이디어 매우 좋음
- class label 없이 채널 정보만 이용하는 방법은?
- 일종의 채널 그룹화 개념 들어간 듯