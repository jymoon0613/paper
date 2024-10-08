# Analyzing and Improving the Image Quality of StyleGAN (CVPR, 2020)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Karras_Analyzing_and_Improving_the_Image_Quality_of_StyleGAN_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/karras2020analyzing.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 high-resolution image 생성의 SOTA인 StyleGAN을 분석하여 artifacts를 수정하고 퀄리티를 높이는 방법에 대해 논의함 (StyleGAN2)

## 접근법
### (1) Removing Normalization Artifacts
- StyleGAN으로 생성한 이미지에는 물방물 모양과 유사한 얼룩이 나타나는 문제가 있음 
- 이러한 얼룩은 64x64의 작은 resolution에서도 나타나며 resolution이 점진적으로 커지는 상황에서 더욱 뚜렷해짐
- 문제의 원인을 AdaIN 연산에서 찾음
- Bias와 noise를 더하는 과정을 style block 밖으로 뺌
- 정규화 과정에서 평균값을 사용하지 않으며 오직 표준편차로 나눔
- 이러한 변경사항은 단일 convolution layer의 가중치를 수정해주는 것만으로 구현 가능
### (2) Image Quality and Generator Smoothness
- 잠재 공간의 perceptual smoothness를 측정하는 path length regularization(PPL)이 이미지 퀄리티와 깊은 상관관계가 있음
- 따라서, 이를 regularization term으로 통합함
### (3) Progressive Growing Revisited
- Progressive growing은 high-resolution 이미지 생성을 학습하기 위해 generator와 discriminator를 점진적으로 추가하는 방식
- 하지만, progressive growing을 사용할 때 generator는 이미지의 details에 대해 location을 유지하도록 하는 경향이 있음 (e.g. 얼굴이 회전하는데 치아의 배열은 정면의 것을 유지함)
- 이는 추가되는 layers로 인해 발생하는 문제임
- 따라서, progressive growing을 사용하지 않고 skip-connection, residual nets을 사용하는 방식으로 generator와, discriminator의 구조를 변경함
### (4) Projection of Images to Latent Space
- 학습 과정에서 더 복잡한 latent space를 찾기 위해 latent vector에 rampled-down noise를 더함
- Stochastic noise를 최적화함

## 실험결과
- 위 변경사항은 기존 StyleGAN의 문제점들을 해결하였고 생성 퀄리티를 더욱 개선함

## 의견
- /