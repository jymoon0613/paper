# CBAM: Convolutional Block Attention Module (ECCV, 2018)

[논문 링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Sanghyun_Woo_Convolutional_Block_Attention_ECCV_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/woo2018cbam.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 네트워크의 표현력 증가를 위해 Depth, Width, Cardinality가 높아야 하지만, Attention을 통해 표현력을 증가시킬 수도 있음 
- Channel과 Spatial 정보에서 중요도를 스케일링 할 수 있는 Attention Module 제안 
- CBAM은 네트워크의 표현력과 성능을 증가시킴 
- BAM과 같이 Channel Attention과 Spatial Attention을 병렬로 처리하는 것보다, 순차적으로 처리하는 것이 더욱 효과적이었음 

## 접근법
### (1) Convolutional Block Attention Module 
- Channel Attention과 Spatial Attention은 별도의 Branch로 구분된 것이 아니라 순차적으로 수행됨 
- VGGFace2라는 거대한 Face Recognition 데이터셋으로 Backbone Network를 Pre-training 함 
### (2) Channel Attention Module 
- 입력 Feature Map으로부터 Average Pooling과 Max Pooling을 각각 수행 (채널 방향) 
- 두 출력은 같이 Dense Layer로 입력됨 
- Dense Layer의 출력은 각 입력과 같은 차원이며, 두 벡터의 합을 Sigmoid하여 Channel Attention Map으로 사용 
### (3) Spatial Attention Module 
- 입력 Feature Map으로부터 Average Pooling과 Max Pooling을 각각 수행 (채널 방향) 
H X W X C → H X W 
- 두 출력은 Concatenate되고 7 X 7 Convolution을 진행 + Sigmoid하여 Spatial Attention Map 도출 

## 실험결과
- ImageNet, COCO 등의 데이터셋에서 성능 검증 
- Grad-CAM으로 모델 학습 수준 시각화 

## 의견
- 더욱 개선할 방법은? 
- Multihead Attention으로 구성? 
