# Conditional Generative Adversarial Nets (arXiv, 2014)

[논문 링크](https://arxiv.org/abs/1411.1784)

<p align="center">
    <img width="400" alt='fig1' src="../img/mirza2014conditional.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- GAN은 여러 intractable한 확률적 계산을 하지 않고도 생성 모델을 훈련시킬 수 있는 프레임워크를 제안함
- 본 연구에서는 기존 GAN에 조건 (condition)의 개념을 추가하여 생성되는 데이터를 임의로 조작할 수 있도록 하는 방법을 제안함 (Conditional GAN (CGAN)) 

## 접근법
- GAN 모델의 generator 및 discriminator에 condition을 의미하는 추가적인 정보(y)를 입력으로 추가해주며 conditional한 generative model로 확장가능함
- 추가 정보 y로는 class label이 될 수도 있고 다른 modalities로부터의 정보가 될 수도 있음
- Generator와 discriminator 각각에 y를 입력으로 사용하기 위한 추가적인 input layer 설계
- Generator의 입력으로는 기존의 input noise와 추가 정보 y를 concat한 것을 사용
- 마찬가지로 discriminator의 입력으로는 이미지와 추가 정보 y를 concat한 것을 사용 (실제 이미지 + y, 생성 이미지 +y)

## 실험결과
- MNIST 데이터셋에서 condition 변수 y로 class label을 사용하여 학습
- y인 class label을 변경하면서 이미지 생성 시 해당하는 class의 숫자 이미지가 생성되는 것을 확인 (conditioned generative model)

## 의견
- /