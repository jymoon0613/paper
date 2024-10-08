# Context Encoders: Feature Learning by Inpainting (CVPR, 2016)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Pathak_Context_Encoders_Feature_CVPR_2016_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/pathak2016context.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 자연의 시각적 세계는 매우 구조화되어 있음 
- 인간은 주어진 시각적 뷰에 대해 전체적인 구조를 이해하여 생각하기 때문에, 어떤 이미지의 일부분이 가려져 있어도 소실된 부분을 떠올리거나 그릴 수 있음 
- 따라서, 본 연구에서는 CNN 구조를 이용하여 이러한 인간의 시각 구조를 모방하고 모델이 주어진 이미지의 전체적인 구조를 이해할 수 있도록 하는 훈련 방법을 제안함 (Context Encoder)
- 구체적으로, 모델은 일부 가려진 이미지를 입력으로 받아 가려진 부분을 생성해내도록 훈련됨 
- 제안하는 모델의 구조는 Autoencoder와 매우 유사하지만, Autoencoder는 이미지 일부를 가리는 등의 제약이 없으므로 차원 압축 모델에 가깝고 의미론적 학습을 한다고 보기 어려움 
- Denoising Autoencoder 또한 일부 노이즈를 섞어 제약을 주긴 하지만 의미론적 학습을 위해서는 제약이 불충분함 
- 반면에 Context Encoder는 가려진 부분에 대한 Pixel-level 생성 Task를 수행해야 하므로, 전체 이미지에 대한 구조적 이해와 가려진 부분에 대한 강력한 가정 생성을 학습할 수 있어야 함 (의미론적 학습에 적합) 

## 접근법
### (1) Encoder-Decoder Pipeline 
- Decoder가 전체 구조에 대해 추론할 수 있도록 Channel-wise Fully Connected 구조로 설계하는 것이 중요함 (일반적인 FC Layer보다 파라미터 수도 훨씬 적음) 
- AlexNet 구조를 Encoder로 사용 
- Decoder 구조는 5개의 연속적인 Up-Convolution 구조 
### (2) Loss Function 
- L2 Loss를 사용하여 Pixel-level 차이를 비교하는 것은 불충분 
- Discriminator를 사용하여 Adversarial Loss를 추가로 사용 
- 두 Loss Function을 같이 사용할 때 더 실제 같은 이미지 생성했음 
### (3) Region Masks 
- 항상 이미지의 중앙을 가리는 Central Region 방식은 Generation에는 유리하지만 표현 학습 성능은 낮았음 
- 무작위한 위치에 Mask를 생성하는 Random Block은 Central Region 방식에 비해 적절하지만 여전히 Block의 경계 정보 학습으로 모델의 표현 학습 성능이 좋지 못함 
- Random Region 방식으로 사각형이 아닌 Random한 모양으로 이미지를 가리는 것이 표현 학습에 가장 좋았음 (Mask의 모양 또한 무작위) 

## 실험결과
- SOTA 성능은 아니지만 이미지 분류에서 좋은 성능 기록 
- Object Detection 같은 다른 Downstream Tasks에도 적용 

## 의견
- Generative 기반의 SSL 방법 
- Pretraining으로서의 성능 개선이 크지는 않음 
