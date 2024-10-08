# A MultiPath Network for Object Detection (BMVC, 2016)

[논문링크](https://arxiv.org/abs/1604.02135)

<p align="center">
    <img width="800" alt='fig1' src="../img/zagoruyko2016multipath.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Detection task을 위한 데이터셋으로 PASCAL VOC와 COCO가 주로 사용됨
- 이 중 COCO는 훨씬 challenging한 데이터셋으로 자리매김하고 있음
> - (i) Objects가 매우 다양한 scale로 존재함 (small objects의 비율도 큼)
> - (ii) Objects는 여러 configurations로 나타날 수 있고 objects 간 겹침이 심하거나 occlusion이 발생하는 경우도 많음
> - (iii) 평가지표는 object localization의 정확도에 민감함
- 본 논문에서는 Fast R-CNN을 베이스라인으로 하여 여러 변화를 주면서 COCO 데이터셋에서의 성능 향상을 이끌어내고자 함
> - Multi-stage feature aggregator, foveal structure in the classifier network, novel loss function
> - 이러한 세 가지 변화는 여러 object views를 포함하는 network 수준의 정보가 여러 paths, layers로 흐를 수 있도록 함 (multipath network, MPN)

## 접근법
- Detection task에서 context 정보는 중요하지만 기존의 Fast R-CNN은 이를 고려하지 않고 하나의 classifier가 object proposal 안의 features만 추출하여 사용함
- 이에 MPN은 classifier head를 여려 개 사용하여 서로 다른 크기의 object proposals에서 features를 추출하도록 함 (foveal structure)
> - 4개의 classifier head를 사용하며, 각각은 주어진 region proposal을 1x, 1.5x, 2x, 4x 배 한 region proposal 내의 features를 추출함
> - RoI-pooling을 사용하여 4개의 classifier head로부터 같은 크기의 features가 추출되며, 모든 4개의 features는 하나의 feature vector로 concat됨
> - 이후 concat된 single feature vector가 object classification 및 bbox regression에 사용됨
- 또한, 기존의 Fast R-CNN은 detection 과정에서 VGG-16의 마지막 feature maps만 사용하는데, 이는 많은 downsampling으로 인해 대부분의 spatial information이 소실될 수 있음
> - Small objects를 효과적으로 localize하기 위해서는 훨씬 큰 resolution의 feature maps가 필요함
- 이에 MPN은 VGG-16의 마지막 3개 layers의 feature maps를 detection 과정에서 모두 사용함
> - ION과 거의 유사함
> - 4개의 classifier head는 각각 주어진 scale의 object proposal에 대해 3개의 서로 다른 scale의 feature maps로부터 features를 추출함 (RoI-pooling을 통해)
> - 각 classifier head는 추출된 3개의 features를 모두 사용하여 하나의 output을 계산함
- 마지막으로, COCO의 평가지표는 object localization 정확도에 민감하게 정의되어 있지만, Fast R-CNN의 loss function은 일반적인 수준의 object localization만 요구하는 PASCAL VOC의 평가지표에 최적화되어 있음
> - PASCAL VOC의 경우 ground-truth bbox와의 IoU가 0.5 이상인 경우의 AP($AP^{50}$)만 고려하지만, COCO는 IoU threshold를 50에서 95까지 변경하면서 평균적인 AP를 고려함
> - 이때 IoU가 큰 bbox에 더 큰 AP를 부여하기 때문에 COCO의 평가지표는 localization 정확도에 더 민감하다고 볼 수 있음
> - 반면, PASCAL VOC는 IoU가 0.5 이상이기만 하면 되므로 기본적인 성능의 object localization만 요구함
> - Fast R-CNN은 training 시 fg/bg IoU를 0.5로 설정하고 있고, 이러한 고정된 IoU threshold($u$)를 사용하는 것은 특정 IoU에 대한 AP($AP^{u}$)를 개선해주지만, 다른 IoU에 대한 AP는 감소시키는 문제가 있음 (COCO metric에 적합하지 않음)
- 이에 MPN은 여러 IoU thresholds에서의 성능을 전체적으로 향상시킬 수 있는 loss function을 사용함
> - 50에서 75까지(6개) IoU를 5씩 증가시켰을 때($AP^{50} - AP^{75}$)의 Fast R-CNN loss를 평균한 loss를 사용함
> - 6개 IoU 각각에 서로 다른 classifier를 사용하여 예측함
> - Inference 시에는 6개 IoU에서의 class score 예측 값을 평균한 값을 최종 class score 예측값으로 사용함

## 실험결과
- COCO object detection task에서 두 번째로 좋은 성능 달성
- 특히 small-sized objects에 대한 정확도가 크게 증가함

## 의견
- / 