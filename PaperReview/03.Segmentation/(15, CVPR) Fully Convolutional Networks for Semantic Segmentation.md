# Fully Convolutional Networks for Semantic Segmentation (CVPR, 2015)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2015/html/Long_Fully_Convolutional_Networks_2015_CVPR_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/long2015fully.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN은 recognition에서 좋은 성능을 기록하였으며, object detection, semantic segmentation 분야에서도 활발히 적용됨
- 본 논문에서는 pixel by pixel로 예측을 수행하며 end-to-end로 훈련하는 fully convolutional network (FCN) 구조를 제안함 
- 기존의 pre-trained된 classification 모델을 pixel 수준 예측에 적합한 fully convolutional 구조로 만들고 fine-tuning하여 사용함
- 또한, skip connection을 통해 local 정보와 global 정보의 계층적인 특징을 결합하여 사용하고자 함

## 접근법
- 기존의 fully connected networks는 입력의 크기가 고정되어 있어야하며 spatial한 정보를 소실함
- 이러한 구조를 fully convolutional한 구조로 변경함으로써 다양한 입력을 처리할 수 있고, spatial 정보를 잘 보존하며, 연산량 측면에서도 효율적인 네트워크 설계 가능함
- 일반적인 recongtion model은 연산이 진행될수록 feature map의 크기가 작아짐 (coarse)
- 하지만 sementic segmentation을 위해서는 픽셀 단위 예측을 정확하게 수행할 수 있을 만큼의 큰 feature map (dense map)이 필요함
- 이를 위해 네트워크의 pooling을 사용하지 않거나 pooling의 stride를 줄이는 방법이 있음
- 하지만, 이러한 방식은 receptive field의 감소로 global context 모델링이 어려워진다는 점과, pooling의 중요한 역할 중 하나인 파라미터 수를 줄이는 효과를 누리지 못한다는 단점이 있음
- 따라서, coarse map에 bilinear interpolation을 적용하는 방식과 backward convolution (deconvolution)을 적용하는 방식을 통해 dense map으로 변환함
- 또한, 하위 layer의 정보를 skip connection으로 결합하여 더 finer한 예측이 가능하도록 함

## 실험결과
- Semantic segmentation task에서 SOTA 성능 달성

## 의견
- /