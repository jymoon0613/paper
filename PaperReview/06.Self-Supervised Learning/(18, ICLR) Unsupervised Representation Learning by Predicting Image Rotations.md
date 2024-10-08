# Unsupervised Representation Learning by Predicting Image Rotations (ICLR, 2018)

[논문 링크](https://arxiv.org/abs/1803.07728)

<p align="center">
    <img width="600" alt='fig1' src="../img/gidaris2018unsupervised.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근의 CNN은 깊은 학습 능력을 보여주었지만, 이는 엄청나게 많은 양의 Labeled 데이터를 요구함 
- 따라서 CNN의 표현 학습을 Unsupervised한 방식으로 하고자 하는 움직임이 나타남 (Self-supervised) 
- 본 연구에서는 모델이 이미지의 지리적 변형 (회전 정도)를 예측하여 표현 학습을 하도록 함 
- 모델이 이미지에 적용된 회전 변환을 인식하려면 이미지에 존재하는 객체, 해당 객체의 유형 및 포즈에 대해 잘 학습해야 함 
- 즉, 핵심은 이미지에 묘사된 핵심 객체의 개념을 인식하지 못하면 회전 인식이 불가능할 것이라는 아이디어 

## 접근법
- 0, 90, 180, 290의 회전 변환을 각각 적용 
- 모델은 회전 변환된 이미지의 Label을 예측 (e.g. 90도 = 1) 
- 모델이 회전 변환을 잘 예측하기 위해서는 이미지에 존재하는 Object의 위치, 성질, 포즈 등을 잘 학습해야 함 

## 실험결과
- ImageNet, CIFAR 데이터셋에서 좋은 성능을 보임 

## 의견
- 개선 방안 생각해보기