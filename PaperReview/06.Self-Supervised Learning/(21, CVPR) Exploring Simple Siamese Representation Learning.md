# Exploring Simple Siamese Representation Learning (CVPR, 2021)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2021/html/Chen_Exploring_Simple_Siamese_Representation_Learning_CVPR_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/chen2021exploring.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Siam Networks란 두개 이상의 입력에 대해서 가중치를 공유하는 것을 말함 
- 기존의 Siamese Networks를 이용한 SSL 방법(이미지의 유사도 기반 학습)들은 출력이 하나의 상수로 붕괴되어 버리는 문제가 있었음 
- 이를 해결하기 위해 SimCLR, Clustering 등의 다양한 전략이 사용되었음 
- 본 연구에서는 추가적인 전략을 사용하지 않고 Simple Siamese Networks를 사용하여 Representation Learning을 수행하는 방법을 제안함 (SimSiam) 

## 접근법
- 하나의 입력 이미지로부터 Augmented된 이미지 2개를 입력으로 받음 
- 두 이미지는 Backbone 네트워크와 Projection MLP Head로 구성된 인코더를 통해 처리됨 
- 인코더는 두 이미지를 처리할 때 같은 가중치를 공유함 
- 추가로 구성된 Predictor는 인코더의 출력을 받아 처리함 
- P1 = Encoder(Predictor(x1)), Z2 = Encoder(x2) 
- Loss 함수는 P1과 Z2의 Negative Cosine Similarity를 최소화하는 것 (최소값 = -1) 
- Z1 (or Z2)는 Stop Gradient로 처리 (상수로 취급) 

## 실험결과
- Stop Gradient의 결과 Collapsing 현상 해결 
- 좋은 Representation Learning 성능을 보임 

## 의견
- 수정할 방법 생각해보기