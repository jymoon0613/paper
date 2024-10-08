# Unsupervised Visual Representation Learning by Context Prediction (ICCV, 2015)

[논문 링크](https://www.cv-foundation.org/openaccess/content_iccv_2015/html/Doersch_Unsupervised_Visual_Representation_ICCV_2015_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/doersch2015unsupervised.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이미지로부터 생성된 여러 패치의 위치를 예측하는 Self-supervised Learning Task를 제안 
- 모델이 Patch Prediction Task를 잘 수행하기 위해서는 이미지의 Context와 해당 이미지에 존재하는 Object에 대한 이해가 필요함 
- 즉, 해당 Task를 통해 모델은 입력 이미지에 대한 Context를 학습함 (Representation Learning) 

## 접근법
- 나누어진 각각의 패치로부터 Backbone CNN 네트워크를 거쳐 피처 벡터를 추출 
- 각 이미지로부터 96x96의 Resolution으로 패치를 생성 
- 모델은 모든 패치의 위치 (# of Class = 8)를 예측함 

## 실험결과
- Image Classification과 Object Detection 모두에서 검증 

## 의견
- 너무 오래된 방식 
- 패치 예측 기반의 방식은 효과가 있을 것 같음 