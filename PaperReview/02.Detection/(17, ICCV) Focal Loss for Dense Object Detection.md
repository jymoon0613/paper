# Focal Loss for Dense Object Detection (ICCV, 2017)

[논문링크](https://openaccess.thecvf.com/content_iccv_2017/html/Lin_Focal_Loss_for_ICCV_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/lin2017focal.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 two-stage detector의 성능에 매우 가까운 one-stage detector를 훈련시키는 것을 목표로 함
- One-stage detector의 성능은 주로 class imbalance 문제에 의해 제한됨
- Class imbalance 문제: 일반적으로 이미지 내의 객체 수가 적기 때문에 positive samples(객체 영역)이 negative samples(배경 영역)에 비해 매우 적음
> - class imbalance 문제로 인해 아주 쉬운 easy negatives가 많이 생성되어 모델이 class를 학습하는 데 유용한 기여를 하지 못함
> - 또한, easy negatives의 수가 압도적으로 많기 때문에 학습에 미치는 영향이 커져 모델의 성능이 하락함
- Class imbalance 문제는 two-stage detector의 경우에는 발생하지 않음
> - Region proposal 과정이 명시적으로 존재하여 불필요한 background 정보를 걸러냄
> - Sampling heuristic을 통해 positive/negative samples의 수를 적절하게 유지함
- One-stage detector의 경우 region proposal 과정 없이 전체 이미지를 순회하면서 dense하게 sampling하므로 훨씬 더 많은 cadidates 영역이 생성되고, 이는 심각한 class imbalance 문제를 야기함
- 또한, sampling heuristic을 적용하여도 easy negatives가 압도적으로 많기 때문에 학습이 비효율적임
- 따라서, 이러한 class imbalance 문제를 해결하고 two-stage에 버금가는 one-stage detector를 훈련시킬 수 있는 새로운 loss function(focal loss)를 제안함
> - Focal loss는 dynamic하게 cross-entropy를 scaling함
> - Focal loss에서 사용되는 scaling factor는 easy examples에 대한 contribution을 자동으로 낮추고 hard exmaples에 대한 모델의 집중도를 올림
- 제안하는 focal loss의 효과를 검증하기 위해 간단한 one-stage detector인 RetinaNet을 디자인함

## 접근법
- 기존의 cross-entropy loss(CE)는 쉽게 classify할 수 있는 examples에 대해서도 같은 크기의 loss를 발생시킴
> - 만약 많은 easy examples가 있다면, 모델이 일부 rare classes 를 분류하는 것을 매우 어려워하더라도 많은 낮은 loss values에 의해 가려질 수 있음
- 이를 해결하기 위한 가장 쉬운 방법은 클래스마다 가중치를 두는 것(ex. class frequency 기반의 가중치) (balanced cross-entropy)
- 하지만 balanced cross-entropy는 positive/negative sample 사이의 균형을 잡아줄 수 있지만, easy/hard sample에서의 균형은 반영하지 못함
- 따라서, easy example을 down-weight하여 hard negative sample에 집중하도록 하는 focal loss를 정의함
- Focal loss는 tunable focusing parameter인 $\gamma$를 사용하는 modulating factor($(1-p_{t})^{\gamma}$)를 CE에 추가한 형태
> - Example이 잘못 분류되고, $p_t$가 작으면, modulating factor는 1과 가까워지며, loss는 영향을 받지 않음
> - 반대로 $p_t$가 크면 modulating factor는 0에 가까워지고, 잘 분류된 example의 loss는 down-weight됨
> - focusing parameter $\gamma$는 easy example을 down-weight하는 정도를 부드럽게 조정함
- 실제 구현에서는 기존의 focal loss에 balance weight(of balanced cross-entropy)를 추가한 형태를 사용함
- RetinaNet은 하나의 backbone network와 각각 classification과 bounding box regression을 수행하는 2개의 subnetworks로 구성
> - ResNet 기반의 FPN을 backbone으로 사용함
> - 두 개의 subnetworks는 fully-convolutional한 구조로 디자인함

## 실험결과
- COCO 데이터셋에서 실험했을 때 focal loss는 CE, balanced CE보다 AP가 높았음
- 또한 focal loss는 class imbalance 문제 해결에 있어서 더 뛰어났음
- 또한 대표적인 two-stage detector인 Faster R-CNN보다 높은 성능을 보여줌

## 의견
- /