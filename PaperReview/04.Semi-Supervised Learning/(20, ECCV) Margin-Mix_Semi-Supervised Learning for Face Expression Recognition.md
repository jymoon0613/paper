# Margin-Mix: Semi-Supervised Learning for Face Expression Recognition (ECCV, 2020)

[논문 링크](https://link.springer.com/chapter/10.1007/978-3-030-58592-1_1)

<p align="center">
    <img width="600" alt='fig1' src="../img/florea2020margin.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Label 데이터 수집이 힘든 FER을 위해서는 labeled 데이터에 대한 의존도를 낮추는 방식으로 학습하는 방식에 대한 고려가 요구됨
- 따라서, 본 논문에서는 FER의 Label Dependency를 줄이기 위해 Classical Self-labelling Paradigm에 기반한 Semi-supervised Learning 방법을 제안함 

## 접근법
- Center loss는 임베딩된 피처들이 해당하는 클래스 피처 클러스터의 중심과 가까워지도록 함으로써 명시적으로 intra-class variations를 줄이는 방법임
- 본 연구에서는 기존의 center loss에 feature normalization 과정을 추가하고, inter-class variations을 최대화하는 term을 추가함으로써 기능을 강화
- 입력으로 unlabeled 데이터가 들어오면 해당 입력의 임베딩 피처와 모든 클래스 클러스터 중심 피처와 거리를 계산하여 가장 가까운 클러스터의 클래스를 할당 (self-labeling)
- Self-labeling으로 레이블이 지정된 unlabled 데이터는 기존의 labeled 데이터와 함께 학습에 사용됨

## 실험결과
- Labeled 데이터가 제약적인 상황에서 좋은 성능

## 의견
- /