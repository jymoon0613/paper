# MaskTune: Mitigating Spurious Correlations by Forcing to Explore (NIPS, 2022)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2022/hash/93be245fce00a9bb2333c17ceae4b732-Abstract-Conference.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/asgari2022masktune.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Spurious correlations은 input과 output간의 잘못된 상관관계가 형성되는 것을 의미함
> - Data selection bias 등으로 인해 발생할 수 있음
> - Training data에서 spurious correlations이 발생하게 되면, domain shift가 발생하거나 test data에서는 그러한 correlations이 없는 경우에 엄청난 성능 하락이 발생할 수 있음
- 예를 들어 소와 낙타를 구분하는 image classification에서 spurious correlations이 발생할 수 있음
> - Data selection bias로 소 이미지는 녹색 초원 background를 주로 가지며, 낙타 이미지는 사막 background를 주로 가진다고 가정
> - 이 경우, 모델은 소와 낙타 분류에 있어서 object보다 background에 더 집중할 수 있음
> - 즉, image와 class label 간의 잘못된 상관관계가 생성됨 = spurious correlations
> - 이는 배경이 없는 소와 낙타 이미지를 분류할 때 모델 성능이 매우 하락할 것임을 의미
- 본 논문에서는 모델이 spurious correlations을 학습하는 것을 방지하기 위한 single-epoch finetuning technique인 MaskTune 기법을 제안함

## 접근법
- 기본적으로 모델은 정의된 prediction loss가 최소화되도록 훈련됨
- 또한, 본 논문에서는 prediction loss 최소화를 넘어 모델이 spurious features에 편향되지 않도록 함
- 이를 위해 training data를 masking하여 새로운 training set을 만들고, 학습이 완료된 모델을 새로운 masked training set에서 fine-tuning하는 방법을 사용
> - Spurious features에 대한 over-reliance를 감소시킴
> - 작은 learning rate으로 단 1 epoch만 훈련시킴
> - Learning rate이 커지거나 1 epoch를 넘어가는 훈련량은 모델이 이전에 학습한 discriminative features를 forget하여 성능이 하락하는 결과로 이어졌음
- Masking은 학습이 완료된 모델에 대해 offline으로 수행됨
> - xGradCAM으로 activation map을 얻고, activation map을 기준으로 가장 activation된 spatial locations 일부를 마스킹함
> - Masking된 입력으로 구성된 training set에서 1 epoch 동안 fine-tuning함
> - 이는 모델로하여금 기존에 집중했던 features 외의 새로운 features를 발견할 수 있도록 장려함

## 실험결과
- Spurious Features를 강제로 추가한 MNIST 데이터셋을 직접 구축하고 평가함
> - 0-4는 class 0으로 재배정하고, 5-9는 class 1으로 재배정함
> - 이후, class 0는 99%의 이미지 왼쪽 상단에 파란색 정사각형 노이즈를 추가하고, class 1은 1%의 이미지 왼쪽 상단에 해당 노이즈를 추가함
> - Test 시에는 class 1에 속하는 이미지 전부의 왼쪽 상단에 파란색 정사각형 노이즈를 추가한 biased test set 사용
> - Original MNIST test set도 사용함
- MaskTune을 사용했을 때 유사한 기법인 RandMask나 일반 분류 모델보다 original test set, biased test set 모두에서 성능이 좋았음
> - 나머지 두 방법은 spurious correlations을 해결하지 못했음

## 의견
- / 