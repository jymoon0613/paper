# Cross-Model Pseudo-Labeling for Semi-Supervised Action Recognition (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Xu_Cross-Model_Pseudo-Labeling_for_Semi-Supervised_Action_Recognition_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/xu2022cross.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Supervised learning에서는 많은 양의 labeled 데이터가 필요하지만, 현실에선 거의 불가능함 
- 하지만 unlabeled 데이터는 비교적 쉽게 구할 수 있음 
- 이러한 unlabeled 데이터를 최대한 활용하는 것이 좋음 
- 가장 대표적인 방법은 unlabeled 데이터에 임의의 가짜 레이블 (pseudo label)을 지정하여 이를 마치 labeled 데이터인 것처럼 훈련에 사용하는 것 
- 대부분의 연구에서는 labeled 데이터로 학습된 분류기가 unlabeled 데이터의 label을 예측함 
- 하지만, 적은 labeled 데이터로 학습된 분류기는 매우 부정확할 수 있고, 따라서 불확실한 pseudo-label을 생성할 수 있음 
- 따라서, 본 연구는 정확한 pseudo-labeling을 위한 접근법을 제시하고 있음 
- Motivation으로, 연구진들은 서로 다른 크기의 모델은 주어진 액션 비디오의 서로 다른 측면에 집중하고 있음을 발견함: 작은 모델의 경우 temporal dynamics, 큰 모델의 경우 spatial semantics 
- 이는 만약 작은 모델과 큰 모델을 같이 pseudo-labeling 과정에 사용한다면 각자가 강점이 있는 액션 종류에 대한 상호 보완적인 label 지정이 가능할 것임 
- 본 연구에서는 Cross-Model Pseudo-Labeling (CMPL) 방법을 제안함: primary backbone은 서로 구조가 다르고 더욱 가벼운 네트워크와 협업 
- 이러한 방식은 이전의 방식들보다 퀄리티 높은 pseudo-labeling 결과를 이끌어냄

## 접근법
- Pseudo-labeling의 목적은 가능한 많은 high quality pseudo-labels를 만드는 것 
- 하지만, 학습 가능한 labeled 데이터가 매우 적다면, labels를 생성하는 분류기의 정확도도 매우 낮아짐 
- 따라서, 본 연구의 접근법은 서로 다른 구조의 네트워크를 학습시켜 서로가 서로에게 pseudo-labels를 생성하도록 함 (Primary backbone, Auxiliary network) 
- 이는 두 네트워크가 서로의 상호 보완적인 representations을 학습하도록 함 
- 제한적인 labeled 데이터로 효과적으로 학습한 모델은 더욱 많은 high quality pseudo-labels를 생성하게 되고, 이는 결국 semi-supervised learning의 성능 향상으로 이어짐 
- Auxiliary network는 weakly augmented view에 대한 예측을 수행하고, 예측된 category-wise probabilities가 특정 threshold를 넘을 경우, 해당 category(class)를 strongly augmented view의 label(pseudo-label)로 사용하여 primary backbone을 훈련 
- 같은 방식으로 auxiliary network도 훈련 
- Inference는 오직 primary backbone만 사용 

## 실험결과
- Baseline model인 FixMatch의 성능을 크게 개선

## 의견
- 제안하는 방법은 추가적인 input 없이 raw RGB 비디오만 사용한다는 점에서 의의가 있음 
- Figure나 수식 등 매우 깔끔함 
- Ablation study, empirical analysis를 통해 제안하는 방법에 대해 자세하게 분석함 
