# Unseen Object Amodal Instance Segmentation via Hierarchical Occlusion Modeling (ICRA, 2022)

[논문링크](https://ieeexplore.ieee.org/abstract/document/9811646)

<p align="center">
    <img width="800" alt='fig1' src="../img/back2022unseen.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 보이지 않는 물체에 대한 segmentation은 unstructured environments에서의 robotic manipulation을 위해 매우 중요함
- 최근에는 large-scale data로부터 objectness를 학습하여 category-agnostic한 instance segmentation을 수행하는 방법들이 많이 제안되었지만 이들은 오직 visible regions에만 집중한다는 한계가 있음
- 인간은 가시적인 정보를 바탕으로 전체적인 구조를 추론할 수 있음 (amodal perception)
- 이러한 amodal perception 기능을 학습시키면 모델은 occlusion이 빈번하게 발생하는 상황에서도 원활하게 manipulation할 수 있을 것임 
- 본 논문에서는 unseen object amodal instance segmentation (UOAIS)을 제안하여 category-agnostic instance segmentation에 amodal 기능을 추가함

## 접근법
- RGB-D 이미지를 입력으로 받아, feature pyramid network (FPN)을 거쳐 피쳐 추출 
- 이후, region proposal network (RPN)을 거쳐 object의 가능한 위치를 추정 
- 추출된 bbox에 대해 bbox regression 및 foreground classification 진행 
- Object가 있는 bbox에 대해 visible mask, amodal mask, occlusion 여부를 예측
- 이때 hierarchical occlusion modeling을 사용하여 bbox, visible mask, amodal mask, occlusion을 순차적으로 예측하도록 함
> - 예를 들어 visible mask를 예측하기 위해 bbox예측 값과 RoI features를 동시에 사용하며, occlusion 여부를 예측하기 위해서는 RoI features와 함께 이전의 bbox, visible mask, amodal mask 예측값을 모두 사용

## 실험결과
- Amodal setting에서 SOTA 성능을 달성함
- Amodel이 고려되지 않는 category-agnostic instance segmentation setting에서도 SOTA와 근접한 성능을 냈음

## 의견
- / 