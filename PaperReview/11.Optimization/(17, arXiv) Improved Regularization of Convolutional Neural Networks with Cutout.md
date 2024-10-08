# Improved Regularization of Convolutional Neural Networks with Cutout (arXiv, 2017)

[논문링크](https://arxiv.org/abs/1708.04552)

<p align="center">
    <img width="400" alt='fig1' src="../img/devries2017improved.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Deep CNN의 거대한 파라미터 수는 represntational power를 증진시키는 동시에 ovefitting을 유발함
- 이러한 overfitting을 방지하기 위해 computer vision에서는 구현이 쉽고 효과적인 augmentation 기법을 주로 사용함
- 본 논문에서는 또 다른 regularization 기법 중 하나인 dropout의 성질을 띄는 augmentation 기법을 제안함 (Cutout)
- 제안하는 방법은 

## 접근법
- Cutout은 기존의 dropout 기법을 입력 이미지에 적용하는 것으로 볼 수 있으며, 이때 각각의 pixel에 대해 dropout이 적용되는 것이 아닌 특정 연속적 영역 수준에서 적용됨
- 입력 이미지에 고정된 크기의 zero-masking을 진행함
- Cutout은 네트워크가 이미지의 전체 context를 이해하도록 강제하여 특정 visual features에만 집중하는 것이 아니라 전체적인 spatial structure를 이용하도록 함
- Cutout 적용 시 zero-mask가 이미지를 벗어날 수도 있도록 설정할 때 성능이 더 좋았는데, 이는 네트워크가 종종 masking이 거의 되지 않는 full image를 볼 수 있도록 하는 것이 중요하다는 것을 의미함

## 실험결과
- Grid search를 통해 각각의 데이터셋에 적합한 cutout size를 찾음
- 모든 데이터셋에서 cutout을 사용할 때 성능 향상 있었음

## 의견
- /