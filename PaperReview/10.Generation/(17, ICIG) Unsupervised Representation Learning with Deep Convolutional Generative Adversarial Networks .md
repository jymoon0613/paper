# Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks (ICIG, 2017)

[논문 링크](https://arxiv.org/abs/1511.06434)

<p align="center">
    <img width="600" alt='fig1' src="../img/radford2015unsupervised.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Unsupervised한 방식으로 모델에게 좋은 표현을 학습시키는 방법 중 하나는 GAN을 이용하는 방법임 
- GAN을 통해 학습된 Generator와 Discriminator는 이후 Supervised Task에 활용될 수 있음 
- 하지만 GAN은 학습 과정에서 모든 입력에 대해 같은 Output을 출력하는 모델 붕괴에 빠질 수 있음 
- 따라서, 본 연구에서는 거의 모든 환경에서 안정적으로 학습할 수 있는 Convolutional GAN 구조를 제안함 

## 접근법
### (1) Approach and Model Architecture 
- 당시 CNN 구조의 연구 흐름 세 가지를 반영함 
- 먼저, All Convolutional Network로 모델을 구성하여 Pooling 없이 Convolution의 Stride로만 Spatial Downsampling(or Upsampling)을 진행함 
- 두 번째로, Fully Connected Layer를 사용하지 않음 
- 세 번째로, Batch Normalization을 사용함 
### (2) Empirical Validation of DCGANs Capabilities 
- ImageNet 1K 데이터셋에서 DCGAN을 사전 훈련시키고, DCGAN의 Discriminator를 CIFAR-10 이미지 분류에 사용 
- Discriminator의 모든 레이어를 Feature Extractor로서 사용하되, 각 레이어마다 Pooling Layer만을 추가함 
- 성능이 좋았음 
- 즉, GAN 혹은 제안된 DCGAN이 Unsupervised한 표현 학습에 장점이 있다는 것을 증명함 

## 실험결과
- 여러 Datasets을 통해 학습하고 실험 결과를 제시 

## 의견
- 코드 돌려보기 
- Unsupervised Representation Learning으로서 GAN을 사용할 수 있음 
