# Explainable Artificial Intelligence (XAI): An Engineering Perspective (arXiv, 2021)

[논문링크](https://arxiv.org/abs/2101.03613)

<p align="center">
    <img width="600" alt='fig1' src="../img/hussain2101explainable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 XAI를 engineering의 관점에서 보고자 함
- 특정 분야에서는 ML/DL 알고리즘의 예측 결과를 사용하는 것을 매우 조심해야할 필요가 있음
> - ML/DL 시스템의 기능 장애가 심각한 결과를 초래하는 경우 (i.e., 자율주행, 병진단)
- 현재 발전하는 ML/DL 모델들은 보통 explainability, transparency, accountability를 고려하지 않음
- Explainable artificial intelligence(XAI)는 black-box인 AI 알고리즘을 white-box로 전환하기 위한 일련의 기술과 기법들을 의미함
> - AI의 output, variables, parameters, output을 도출하기까지의 과정이 투명하고 설명가능해야 함
- XAI의 관점에서 AI 모델은 다음과 같은 특징을 만족해야 함
> - Explainability: 학습 모델의 '능동적' 특징으로, 모델은 자신의 내부에서 수행되는 프로세스를 명확히 설명할 수 있어야 함
> - Interpretability: 학습 모델의 '수동적' 특징으로, 사용자가 모델을 이해할 수 있도록 해야 함
> - Transparency: 모델에 interface와 같은 요소를 추가하지 않고도 학습 모델을 본질적으로 이해할 수 있는 경우 그 모델은 '투명하다'고 함 (추가 구성요소 없이 explainability와 interpretability한 경우)
- XAI의 또 다른 측면으로는 accountability가 있음
> - 법적인 측면을 나타냄 (i.e., privacy-sensitive data)

## 접근법
- Transparent Models은 두 가지 관점으로 표현될 수 있음
> - Algorithmic transparency: 입력에 대한 모델의 output을 사용자가 얼마나 이해할 수 있는가
> - Level of decomposability: 모델의 각 part에 대한 설명이 얼마나 구체적으로 제공될 수 있는가
- 추가실험이나 시각화 등의 post-hoc explanation techniques로 모델을 설명하려는 노력이 많지만 그것만으로는 모델을 완전히 설명할 수 없음
- AI 모델의 가능한 모든 이해관계자들(stakeholders)를 식별하고 각각에 맞는 explanation과 interpretation을 제공할 필요가 있음

## 실험결과

## 의견
- / 