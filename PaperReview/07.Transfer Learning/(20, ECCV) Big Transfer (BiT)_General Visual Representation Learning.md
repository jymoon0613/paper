# Big Transfer (BiT): General Visual Representation Learning (ECCV, 2020)

[논문링크](https://arxiv.org/abs/1912.11370)

<p align="center">
    <img width="600" alt='fig1' src="../img/kolesnikov2020big.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Deep 네트워크는 성능 향상을 위해 상당한 양의 task-specific한 data를 필요로 함
- 하지만, 모든 task에 대해 상응하는 데이터를 수집하는 것은 매우 expensive함
- 이때 transfer learning이 해결책이 될 수 있음
> - task-specific한 data 및 연산은 pre-training phase로 대체할 수 있음
> - 네트워크는 large generic 데이터셋에서 미리 훈련되고, 학습된 가중치는 이후 tasks에서의 초기값으로 사용됨
> - 이는 더 적은 task-specific 데이터와 더 적은 연산(훈련량)으로도 각 task에 뛰어난 deep 네트워크 학습이 가능하도록 함
- 본 논문에서는 이러한 transfer learning의 컨셉을 기반으로 많은 tasks에서 뛰어난 성능 향상을 가져오는 몇 가지 훈련 트릭(recipe)을 제안함 (Big Transfer)

## 접근법
### (1) Upstream Pre-Training
- Pre-training에서 scale(dataset size, 모델 size)의 영향을 실험하기 위해 3개의 dataset에 대해 3개의 BiT model을 학습함
> - BiT-S: ILSVRC-2021 (1.3M images)
> - BiT-M: ImageNet-21K (14M images)
> - BiT-L: JFT (300M images)
- BiT는 ResNet-152x4을 기반으로 Group Normalization(GN), Weight Standardization(WS)을 사용함
> - Batch Normalization(BN)이 가장 성능이 뛰어난 기법이지만 큰 모델을 작은 per-device batches로 학습할 때 BN은 성능이 안좋았음
> - GN을 WS와 함께 사용했을 때 ImageNet과 COCO에서의 small-batch training 시 성능이 좋았음
> - Weight Standardization : Mini-batch와 관련없이, 가중치 행렬을 표준화

### (2) Transfer to Downstream Tasks
- 매우 간단한 fine-tuning protocol을 제안함
> - 모든 task에 동일한 하이퍼파라미터(학습률 등등)을 사용함
> - 대신 이미지 resolution 및 dataset size에 기반하여 training schedule length, resolution, MixUp의 사용 여부만 dataset 특성에 따라 변경함

## 실험결과
- Transfer 결과 매우 적은 데이터만으로도 엄청난 성능을 보임
- Semi-supervised learning 보다 더 좋은 성능
- Classification 뿐만아니라 여러 task(detection 등)에서도 좋은 성능을 보임
- 작은데이터를 쓰는데, 큰 모델을 사용하면 역효과가 날 수 있음
- 반대로 작은 모델을 쓰는데, 큰 데이터 셋을 쓰는 것도 안좋음
- 큰 데이터에 대해 큰 모델을 사용할 때 성능이 좋았음
- 큰 데이터를 학습키 위해선, 더 긴 시간 학습 해야함
- 높은 weight decay 학습 시 더 빠르게 수렴하나, 최종적으로 더 낮은 성능으로 수렴
- 반대로, 낮은 weight decay 학습 시 수렴은 느리나, 최종적으로 더 좋은 성능으로 수렴

## 의견
- /