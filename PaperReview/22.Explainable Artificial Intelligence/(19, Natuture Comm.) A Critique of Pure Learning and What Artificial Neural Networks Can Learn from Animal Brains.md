# A Critique of Pure Learning and What Artificial Neural Networks Can Learn from Animal Brains (Natuture Comm., 2019)

[논문링크](https://www.nature.com/articles/s41467-019-11786-6)

<p align="center">
    <img width="500" alt='fig1' src="../img/zador2019critique.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 내용요약
- AI는 지금까지 많은 기술적 발전을 이루었지만 '지능'의 측면에서 인간과 동물을 따라잡지는 못하고 있음
> - Language, reasoning, common sense, etc.
- AI는 개나 쥐만큼의 지능을 달성하는 데도 어려움이 있으며 현재와 같이 단순히 모델을 scaling up 한다고 해서 달성될 것 같지는 않음
- 반대로, 만약 개나 쥐만큼의 지능을 달성할 수 있다면 인간의 지능을 모방하는 것도 충분히 가능하다는 것
- 쥐와 인간의 지능 차이보다 현재 AI와 쥐의 지능 차이가 더 큼
- 즉, AI의 궁극적 목표가 인간의 지능을 모방하는 것이라면, 현실적인 목표는 우선 동물(animal)의 지능을 모방하는 것
- 동물은 학습과 함께 내재된 메커니즘(innate mechanism)에 의존하고 있음
> - 내재된 메커니즘은 진화를 거듭하며 뇌를 연결하기 위한 규칙의 형태로 게놈(genome)에 인코딩됨
- 즉, 학습으로는 모방할 수 없는 innate processes가 존재한다는 것
- AI는 '학습(learning)'으로 네트워크의 parameters의 변화시키는 것에 집중
- AI에서의 학습은 입력 데이터에서 구조적 특징을 찾아내고 이를 네트워크의 parameters에 반영하는 것
- 반면 뇌과학에서의 학습은 경험의 결과로 오래 지속되는 행동의 변화를 의미
- 두 개념의 분야에서의 학습 개념의 차이는 지도 학습에서 뚜렷하게 나타남
> - ANN은 새로운 object categories를 학습하기 위해 많은 양의 labeled 데이터가 필요
> - 하지만, 어떤 사람이 새로운 object categories를 학습하기 위해 많은 양의 examples를 참고해야 하는 지도 학습 방법을 사용하지는 않음
> - 만약 인간이 ANN의 지도 학습 방법을 사용한다면 매초마다 질문을 한 번씩 하며 학습하는 것과 같음
- 따라서, 최근에는 labeled data를 요구하지 않는 unsupervised한 학습 패러다임이 ANN에서 주목을 받고 있음
> - 하지만, unsupervised learning이라 할지라도 task에 특화된 가정은 필요하며, 완전히 general-purpose한 unsupervised learning 알고리즘을 설계하는 것이 ANN의 새로운 시대를 열 수도 있음
- 그렇다면 어떻게 동물들은 supervised learning 없이 태어나자마자 지능적으로 행동할 수 있는가?
- 이는 동물들에게 많은 sensory representations와 behavior은 이미 게놈에 내재되어 있기 때문임
- 즉, 동물의 행동 양식은 supervised, unsupervised learning에 의한 학습 결과라기 보다는 태어날 때부터 내재된 것
- 하지만, 타고났다고 해서 모든 동물의 지능(혹은 성능)이 제한되는 것은 아님
> - 태어날 때 지능이 더 낮았던 동물이 학습을 통해 태어날 때 지능이 더 높았던 동물과 유사하거나 더 높은 성능을 보일 수 있음
> - 선천적인 행동과 학습된 행동은 시너지 효과를 내며 상호작용할 수 있음
- 이러한 특징들을 기반으로 ANN의 발전을 가속화할 수 있을 것
> - 동물에게는 innate mechanism이 중요하다는 것은 새로운 문제를 해결하는 ANN도 이전의 관련된 문제에 대한 솔루션을 최대한 이용하려고 시도해야 함을 시사 ('transfer learning')
> - 게놈이 표현이나 행동, 최적화 원칙을 직접적으로 인코딩하지 않고 행동과 표현을 인스턴스화하여 wiring rules와 patterns를 인코딩한다는 것은 AI 시스템의 최적화 대상이 wiring topology 혹은 네트워크 아키텍처라는 것을 시사

## 의견
- /