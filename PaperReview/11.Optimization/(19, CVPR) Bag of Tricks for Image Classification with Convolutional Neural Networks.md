# Bag of Tricks for Image Classification with Convolutional Neural Networks (CVPR, 2019)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2019/html/He_Bag_of_Tricks_for_Image_Classification_with_Convolutional_Neural_Networks_CVPR_2019_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/he2019bag.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Training procedure refinements는 이미지 분류 분야의 정확도 향상에 기여해 왔음
> - Loss function, data preprocessing, optimization의 변화
- 하지만, 대부분의 refinements 기법은 implementation details에 짧게 기술되고, 큰 주목을 받지는 못했음
- 본 논문에서는 complexity는 크게 증가시키지 않고 정확도를 개선시킬 수 있는 이러한 refinements 기법을 탐구해보고자 함

## 접근법
- GPU device에서 더 효율적인 training을 할 수 있는 기법을 먼저 소개함
> - (1) Linear scaling learning rate: large batch size는 mini-batch SGD의 변동성을 줄여줌. 따라서 batch size에 따라 learning rate을 증가시켜 weights가 더 큰 폭으로 변하게 해줌.
> - (2) Learning rate warmup: 학습 초기에 네트워크의 parameters는 랜덤한 값으로 초기화되어 있으며 바로 large learning rate을 적용하게 되면 numerical instability가 발생할 수 있음. 따라서 초기에는 작은 learning rate을 사용하다가 training process가 stable해지면 initial learning rate으로 switch하는 warmup heuristics를 사용 (설정한 특정 epoch에 도달할 때까지 learning rate을 0부터 initial learning rate까지 선형적으로 증가시킴).
> - (3) Zero $\gamma$: 초기 학습 단계에서 Batch Normalization의 scale parameter $\gamma$를 0으로 설정하여 학습을 원할하게 함
> - (4) No bias decay: Batch Normalization의 parameters $\gamma$와 $\beta$에는 weight decay를 적용하지 않음
> - (5) Low-precision training: 기존의 네트워크는 FP32 precision으로 훈련되지만, 최근의 GPU device는 더 낮은 FP16 precision도 처리할 수 있는 향상된 arithmetic logic을 지니고 있기 때문에 속도 개선을 위해 FP16 training으로 변경
> - 위의 기법들을 모두 적용했을 때 속도와 성능면에서 모두 향상됨
- 네트워크에 약간의 구조적 변화를 주는 model tweaks 기법을 소개함
> - ResNet을 중심으로 소개
> - (1) ResNet-B: 정보 손실을 줄이기 위해 기존 ResNet block을 구성하는 convolutional layers의 stride를 미세 조정
> - (2) ResNet-C: 연산량을 줄이기 위해 stem block의 초기 7x7 convolutional layer를 세 개의 3x3 convolutional layer로 변경
> - (3) ResNet-D: ResNet-B와 마찬가지로 정보 손실을 줄이기 위해 ResNet block의 shortcut pathway의 stride를 수정하고, average-pooling을 추가
> 연산량은 약간 증가했고 정확도가 개선됨
- 모델 정확도를 향상시킬 수 있는 4가지 training refinements를 소개함
> - (1) Cosine learning rate decay 사용
> - (2) Label smoothing 사용
> - (3) Knowledge distillation 사용
> - (4) Mixup augmentation 사용
> - 네 가지 refinements를 사용했을 때 성능 향상이 있었음
- 본 논문에서 제안한 tricks를 사용했을 때 detection, segmentation으로의 transfer learning 성능도 개선되었음

## 실험결과

## 의견
- /