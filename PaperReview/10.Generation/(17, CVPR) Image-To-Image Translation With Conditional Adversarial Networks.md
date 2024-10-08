# Image-To-Image Translation With Conditional Adversarial Networks (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Isola_Image-To-Image_Translation_With_CVPR_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/isola2017image.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- NLP의 기계변역과 마찬가지로 입력 이미지를 원하는 출력 이미지로 변환(image-to-image translation)하는 것은 많은 비전 태스크에 도움이될 수 있음 (ex. edge image -> phto image, day image -> night image)
- 가장 간단한 방법으로, 모델이 입력 이미지로부터 원하는 출력 이미지를 생성하되 생성된 이미지와 정답 이미지의 유클리드 거리를 최소화하도록 훈련시킬 수 있음
- 하지만, 유클리드 거리는 모든 픽셀 위치에서 거리를 평균하여 최소화되기 때문에 sharp하고 realistic한 이미지가 아닌 blurry한 이미지가 생성됨
- 이러한 문제를 해결하기 위해 모델에게 '실제 이미지와 구분 불가능할 정도로 realistic한' 이미지를 생성하도록 하는 조건을 주는 방법이 있으며, 이는 GAN이 학습하는 방식과 정확히 일치함
- 즉, 본 논문에서는 GAN을 사용하여 입력 이미지를 원하는 출력 이미지로 변환하는 프레임워크를 제안함 (Pix2Pix)

## 접근법
- Noise vector에서 이미지를 생성하는 기존의 GAN과 달리, Pix2Pix의 generator는 conditional GAN과 비슷하게 입력 이미지 x(condition)와 noise vector를 동시에 입력으로 받아 이미지 y를 생성함
- Discriminator는 입력 이미지 x(condition)와 생성 이미지 y (혹은 실제 결과 이미지)를 입력으로 받아 real인지 fake인지 구분함
- 이러한 conditional한 GAN loss에 pixel-by-pixel L1 distance loss를 더하여 최종 loss를 구성함
- 즉, L1 distance loss는 generator의 생성 이미지와 실제 결과 이미지가 pixel 단위로 유사해지도록 하는 역할을 하며, GAN loss를 통해 blurry한 이미지가 아닌 sharp하고 realistic한 이미지가 생성되도록 함
- Pix2Pix 모델은 high-resolution의 입력을 처리하여 high-resolution의 출력을 생성하며, 입력과 출력은 표면적으로 다르지만 유사한 structure를 지니고 있음 (translation이므로)
- 따라서, U-Net의 구조를 따라 skip-connection을 통해 입력과 출력 간의 정보 공유를 가능하도록 함
- Total loss fuction에서 L1 loss는 입력 이미지와 출력 이미지의 low-frequency 선명도를 개선하도록 함 (pixel 단위의 loss 계산을 통해)
- 따라서, GAN의 discriminator는 low-frequency 선명도를 고려할 필요가 없으며 전체적인 완성도나 context 같은 high-frequency structure만을 고려하도록 함
- 생성 이미지의 high-frequency 정보를 모델링하기 위해서는 이미지의 local한 structure에 집중하는 것이 효과적이며, 이를 위해 discriminator 아키텍처를 PatchGAN으로 디자인함 
- 즉, discriminator는 real과 fake를 판단할 때 전체 이미지가 아닌 NxN 단위의 패치 단위로 prediction함 (texture/style을 개선시키는 역할)

## 실험결과
- Labels-photo, edges-photo 등 여러 tasks에 대해서 실험 진행
- Loss function의 다양한 조합에 대해 실험한 결과 L1 loss와 cGAN loss를 같이 썼을 때 가장 높은 성능을 보이며 qualitative 측면에서도 좋았음
- Skip-connection이 없는 일반적인 encoder-decoder 구조를 사용했을 때보다 U-Net과 같이 skip-connection을 사용했을 때 생성 이미지의 quality가 크게 개선되었음

## 의견
- /