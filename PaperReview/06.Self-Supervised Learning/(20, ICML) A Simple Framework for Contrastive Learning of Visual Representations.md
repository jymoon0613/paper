# A Simple Framework for Contrastive Learning of Visual Representations (ICML, 2020)

[논문링크](http://proceedings.mlr.press/v119/chen20j.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/chen2020simple.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Unsupervised하게 visual representations를 학습하는 것은 매우 어려운 일
- 지금까지의 unsupervised visual representation learning은 generative한 방식과 discriminative한 방식으로 나눌 수 있음
- Generative한 방식은 input space에서 pixel 단위의 generation 혹은 reconstruction을 수행하여 학습하지만, 이는 computational cost가 매우 크며 표현 학습에는 크게 필요한 작업이 아닐 수도 있음
- Discriminative한 방식은 기존의 supervised learning과 마찬가지로 onjective functions를 최적화하는 방식으로 학습하지만, 사용하는 input과 label 모두 unsupervised dataset에서 추출하는 방식으로 진행됨
- Rotation prediction과 같은 heuristic한 pretext task를 설계하여 학습을 하기도 하지만 이는 학습된 표현의 generality를 악화시킬 수 있음
- Discriminative한 방법 중 contrastive learning이 최근 좋은 성능으로 각광받고 있음
- 따라서, 본 논문에서는 contrastive learning을 위한 simple framework (SimCLR)을 제안함
- SimCLR은 성능이 매우 좋은 데 반해 별도의 특별한 아키텍처 추가나 memory bank를 사용하지 않는 간단한 구조임

## 접근법
- 전체 모델 아키텍처는 CNN 백본과 encoder로 구성됨
- 입력 이미지 배치에 대해 동일한 크기의 augmented views가 생성되며, 하나의 이미지에 대해 같은 이미지에서 augmented된 이미지와는 유사해지도록 학습하고 나머지 augmented views는 negative samples로 사용하여 서로 멀어지도록 학습함 (MoCo에서 언급한 end-to-end 방식 사용)

## 실험결과
- Data augmentation에 대한 실험결과, data augmentation operations를 어떻게 구성할 것인지가 표현 학습 성능이 매우 중요함을 발견
- Augmentation을 통해 contrastive prediction이 어려워질수록 표현 학습 성능은 극적으로 증가함
- Contrastive learning은 supervised learning보다 augmentation 의존도가 높음
- 백본 모델의 크기가 커질수록 같은 모델의 supervised 훈련 결과와의 성능 차이가 좁혀짐
- 백본만 사용하는 것보다 encoder를 사용하는 것이 성능 향상에 효과적이었으며, linear encoder를 사용하는 것보다 ReLU activation을 추가한 non-linear한 encoder를 사용했을 때 성능이 더욱 좋았음
- 대부분의 benchmark 태스크에서 다른 unsupervised 학습 방식보다 좋은 성능 기록
- 특히, transfer learning에서는 supervised model보다 좋은 성능을 기록하여 transferable한 피처를 잘 학습한다는 것을 증명

## 의견
- 방법과 구조는 매우 간단
- 다양한 실험과 그로부터의 통찰을 세세하게 제시함