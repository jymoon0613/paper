# R-FCN: Object Detection via Region-based Fully Convolutional Networks (NIPS, 2016)

[논문링크](https://proceedings.neurips.cc/paper/2016/hash/577ef1154f3240ad5b9b413aa7346a1e-Abstract.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/dai2016r.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ResNet과 GoogLeNet 같은 fully-convolutional한 구조를 object detection에 바로 적용하는 경우 성능이 classfication만큼 나오지 못했음
- 이는 image classification의 translation-invariance와 object detection의 translation variance 간의 충돌이 발생하기 때문임
> - 이미지 내 객체의 위치가 바뀌더라도 동일한 객체로 인식하는 것이 바람직하기 때문에 image classification 모델은 translation invariance 속성을 선호
> - Object detection 시에는 객체의 위치가 변화하면 이러한 변화를 잘 포착하는 것이 바람직하기 때문에 학습 시 translation variance 속성을 선호
- 대부분의 2-stage detector는 feature를 추출하는 역할을 수행하는 backbone network와 detection을 수행하는 sub-network로 구성
- 이때 backbone network는 image classification에서 pre-trained되어 있으며 bacbkone을 통해 얻는 features는 translation-invariance한 특징을 지니고 있음
- 따라서, detection을 수행하는 sub-network는 위치 정보가 소실된 features를 전달받으므로 적절한 훈련이 이루어질 수 없음
> - 이러한 이해관계의 상충을 translation invariance dilmma라고 부름
- ResNet 논문에서는 위의 문제를 해결하기 위해 backbone의 convolution layers 사이에 RoI pooling layer를 추가함
> - 기존의 backbone을 전부 거친 후 RoI pooling을 적용하는 방식과 차이가 있음
> - Region-specific한 효과를 주어 translation-invariance를 제거하고 detection network가 translation-variance한 처리가 가능하도록 함
> - 하지만 모든 RoI를 개별적으로 detection network에 입력하기 때문에 학습 및 추론 속도가 느림
- 본 논문에서는 object detection을 위한 fully-convolutional 구조를 지니는 region-based fully convolutional network (R-FCN)을 제안함
- R-FCN은 position-sensitive score를 계산하여 translation-variance한 fully-convolutional한 아키텍처를 구현함

## 접근법
- R-FCN은 ResNet-101을 backbone network로 사용하며 (i) region proposal, (ii) region classification으로 구성된 2-stage 구조를 따름
> - R-FCN과 RPN은 같은 backbone을 공유함
- Proposal regions에 대해 R-FCN은 주어진 RoI들을 object categories로 classify함
> - R-FCN은 convolutional layers로만 구성됨
- 보다 구체적으로, 먼저 R-FCN은 ResNet backbone으로부터 얻은 feature maps에 추가적인 convolution 연산을 적용하여 ${k}^2(C+1)$의 channels를 갖는 position-sensitive score maps를 얻음
> - 이는 ${k}\times{k}$로 나뉘어진 spatial 그리드의 각 position에서 $C+1$개의 classes (num. object classes + bg)에 대한 class score를 평가한다는 의미임
> - 예를 들어 $k\times{k}=3\times3$이면 주어진 object category에 대한 top-left, bottom-right 등 9개의 relative position 각각에 대한 class score를 계산하는 것을 의미함
- Score maps에 RPN의 output을 사용하여 RoI pooling을 적용하고, 마지막으로 각 RoI에 대한 scores를 생성하는 position-sensitive pooling을 적용함
> - 마찬가지로, RoI를 ${k}\times{k}$의 bin으로 나누고 position-sensitive score maps 상에서 RoI pooling을 수행함
> - 이때 ${k}\times{k}$의 각 bin은 주어진 region 내에서 position-sensitive score maps의 ${k}\times{k}$의 각 grid에 대해 average pooling을 적용한 것임
> - Position-sensitive RoI pooling을 통해 ${k}\times{k}\times{(C+1)}$의 feature를 추출
> - 마지막으로 ${k}\times{k}$의 bin에 대해 다시 average pooling을 적용하여 $(C+1)$ 차원의 features를 추출함
- Bbox regression을 수행할 때도 위와 비슷한 fully-convolutional한 방식을 사용함

## 실험결과
- 같은 backbone을 사용하는 Faster R-CNN보다 성능이 좋았음

## 의견
- / 