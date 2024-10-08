# Learning Deep Bilinear Transformation for Fine-grained Image Representation (NIPS, 2019)

[논문 링크](https://proceedings.neurips.cc/paper/2019/hash/959ef477884b6ac2241b19ee4fb776ae-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zheng2019learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근의 FGVC 방법들은 피처 채널에 집중하는 경우가 많음
- 채널 간 의존성을 모델링하기 위해 Bilinear Transformation을 사용하지만, 이는 계산량이 많음
- 따라서 본 논문에서는, Bilinear Transformation을 효율적으로 계산하기 위한 Deep Bilinear Transformation Block을 제안함 
- 제안됨 Block은 여러 깊은 네트워크 구조에 쉽게 적용될 수 있음 

## 접근법
### (1) Overview 
- Bilinear Pooling은 채널 간의 상관관계를 모델링하기 위해 고안됨
- Bilinear Pooling의 FGVC에서의 효과는 여러 연구로부터 입증됨 
- 기존의 방식에 비해 계산량을 매우 줄이는 방법을 제안함 
### (2) Semantic Grouping Layer 
- 선행 연구로부터 CNN의 고수준 레이어에서는 의미론적인 정보를 추출함 
- 따라서, Convolutional 채널을 여러 의미론적 단위로 분할할 수 있음 
- 1X1 Convolution을 이용함 
- 같은 그룹에 속하는 채널은 유사한 Spatial 정보를 포함한다는 점을 이용하여 그룹화 진행 
### (3) Group Bilinear Layer 
- 같은 그룹 내의 피처 채널 간의 표현을 향상시키기 위해 내부적으로 Bilinear Transform을 진행 
- Bilinear Pooling의 FGVC에서의 효과는 여러 연구로부터 입증됨 
### (4) Deep Bilinear Transformation Network 
- Bilinear Pooling은 채널 간의 상관관계를 모델링하기 위해 고안됨
- Group Order를 반영하기 위해 Position Encoding 추가 

## 실험결과
- CUB-200-2011, Stanford Cars iNaturalist와 같은 Fine-grained 이미지 분류 데이터셋에서 평가함 

## 의견
- Channel Grouping 이용할 방법 찾기