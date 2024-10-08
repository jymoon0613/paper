# Learning Deep Features for Discriminative Localization (CVPR, 2016)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Zhou_Learning_Deep_Features_CVPR_2016_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhou2016learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Global Average Pooling Layer를 약간 수정하고, Class Activation Mapping (CAM) 기술을 추가함으로써 CNN 네트워크가 클래스 별로 이미지의 어떤 부분에 집중하고 있는지를 확인할 수 있음 

## 접근법
- CNN의 마지막 레이어에서의 처리: CNN → GAP → Softmax 
- GAP를 사용하는 경우 Softmax Layer의 입력으로 들어가는 값은 각 Feature Map 채널의 평균값 
- 이때 GAP에서 Softmax Layer로 향하는 가중치들은 Class Label에 따라 다르게 학습됨 (Fully Connected) 
- 즉, GAP가 적용되기 전의 Feature Map 각 채널을 Softmax Layer의 가중치로 Weighted Sum 함으로써 입력 이미지에 대해 클래스 별로 CNN 모델이 어느 부분에 집중하고 있는지를 확인해볼 수 있음 
- 무엇을 보고 이 Feature Map을 모델이 클래스 C라고 분류했는지 알 수 있음 

## 실험결과
- Object Localization Task 성능을 통해 CAM의 성능을 평가 

## 의견
- GAP를 사용하지 않는 경우 사용할 수 없음 
- GAP를 사용하지 않는 네트워크의 경우 GAP를 추가하고 다시 학습해야 함 
