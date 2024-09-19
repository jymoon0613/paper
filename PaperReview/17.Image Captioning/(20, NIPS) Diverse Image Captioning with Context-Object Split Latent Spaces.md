# Diverse Image Captioning with Context-Object Split Latent Spaces (NIPS, 2020)

[논문링크](https://proceedings.neurips.cc/paper/2020/hash/24bea84d52e6a1f8025e313c2ffff50a-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/mahajan2020diverse.png?raw=true"></br>
    <em><font size=2>Diverse image captioning with our COS-CVAE model.</font></em>
</p>

## 연구목적
- Vision-text task와 같은 cross-domain task에서는 many-to-many relations가 존재함
  - Image captioning의 경우 하나의 이미지에 대해 해당 scene을 설명하는 다양한 caption이 존재할 수 있음
- 이전의 연구에서는 주어진 이미지에 대해 다수의 captions을 생성하기 위한 variational framework가 제안되었음
  - 어떤 이미지가 주어졌을 때의 conditional text distribution을 latent space에 인코딩함
- 하지만, 이러한 latent variable models는 데이터셋에서 미리 paired된 image-text에 대해서만 학습이 진행된다는 한계가 있음
- 본 논문에서는 이러한 한계를 극복하기 위해 factorized latent variable model인 context-object split conditional variational autoencoder(COS-CVAE)를 제안함

## 접근법
- 우선 주어진 데이터셋의 GT caption을 context와 object로 분리함
  - Object caption은 "dog, couch"와 같이 주어진 caption에 존재하는 objects 정보를 포함함
  - Context caption은 "A \<s\> sitting on top of a \<s\>"와 같이 object caption을 "\<s\>"로 대체한 나머지 정보를 포함함
- Faster R-CNN을 사용하여 주어진 image에 대한 visual representations를 추출함
- Visual representations는 context/object captions와 함께 각각 서로 다른 LSTM으로 입력됨
- 이후 attention LSTM을 거쳐 두 context/object latent variables가 fusion되고, sequential하게 caption이 생성됨

## 실험결과
- COCO 데이터셋에서 생성된 caption의 다양성, 정확성 측면에서 개선이 있었음

## 의견
- /