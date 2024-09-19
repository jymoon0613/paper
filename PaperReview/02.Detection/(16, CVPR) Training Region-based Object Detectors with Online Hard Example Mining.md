# Training Region-based Object Detectors with Online Hard Example Mining (CVPR, 2016)

[논문링크](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Shrivastava_Training_Region-Based_Object_CVPR_2016_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/shrivastava2016training.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Object detector는 주로 classification과 유사한 방식으로 훈련됨
> - 먼저 입력 이미지로부터 possible object regions를 찾고 (selective search 등을 통해), 찾은 regions에 기반하여 object class 및 bbox 좌표를 예측
- 하지만 이러한 훈련 방식은 classification에서 주로 발생하는 class imbalance 문제를 유발할 수 있음
> - training set에서 object class를 포함하지 않는 background examples가 object를 포함하는 annotated examples에 비해 비중이 매우 커지게 됨
- 비슷한 문제의 해결책으로 오래전부터 bootstrap(hard negative mining) 방법이 사용되기도 했음
> - 모든 object examples와 약간의 background examples에서 시작하여, 모델의 훈련 과정에서 잘못 분류하는 examples를 점진적으로 background examples에 추가하면서 학습함
- Bootstrap 방법은 효과적이었음에도 불구하고 최근의 Fast R-CNN과 같은 detector들은 bootstrap을 사용하지 않음
- 이는 bootstrap이 최근의 CNN 기반 detector에 적용하기에는 기술적인 한계가 존재하기 때문임
> - Bootstrap은 (i) 일정 iterations 동안 모델을 fix시킨 후 training set에 업데이트할 examples를 찾고, (ii) 모델을 다시 일정 iterations 동안 업데이트된 training set에 대해 학습시키는 반복적인 과정으로 구성됨
> - 이때 (i)을 수행하는 일련의 iterations는 수렴을 위해 매우 많은 optimization step이 필요한 CNN 기반의 detector의 경우 훈련 속도를 매우 느리게 하는 원인으로 작용할 수 있음
- 따라서, bootstrap을 CNN 기반 detector에 적용하기 위해서는 online으로 작동하는 hard example selection(i)을 디자인해야 함
- 본 논문은 이러한 목적으로 novel bootstraping technique인 online hard example mining(OHEM)을 제안함
> - OHEM은 SGD를 약간 변형하여 구현할 수 있으며 효율적인 연산이 가능함

## 접근법
- 본 논문은 Fast R-CNN에 OHEM을 적용함
- Fast R-CNN은 selective search를 통해 주어진 이미지에서 2,000개 정도의 RoIs를 추출하고, 추출된 RoIs 중 ground-truth bbox와 IoU가 0.5 이상인 것은 positive examples로, IoU가 0.1 이상 0.5 미만인 것은 negative samples로 사용함
- 이때 모든 positive/negative samples를 사용하는 것이 아니라 batch size($B$)와 이미지 수($N$)를 고려하여($B/N$) 각 이미지마다 적절한 수의 RoIs를 추출하여 미니 배치를 구성함
- 또한, data imbalance 문제를 해결하기 위해 각 미니 배치 에서 foreground와 background RoIs의 비율이 1:3이 되도록 설정해줌
- 이렇듯 기존의 Fast R-CNN의 RoI sampling 과정에는 많은 heuristics가 포함되어 있으며 이는 실험자의 개입과 시행착오가 많이 필요하다는 것을 의미함
> - OHEM은 이러한 heuristic한 hyperparameters를 제거해주는 효과도 있음
- OHEM은 이미지에서 추출한 모든 RoIs를 forward pass한 후 loss를 계산하여, 높은 loss를 가지는 RoI에 대해서만 backward pass를 수행
> - 먼저 주어진 이미지로부터 conv. feature maps를 계산함
> - 다음으로 selective search로 찾은 모든 RoIs에 대해 계산된 feature maps에 RoI pooling을 적용함
> - FC layer, classifier, bbox regressor를 거쳐 각 RoI별로 loss를 계산함
> - Loss에 따라 RoIs를 정렬한 후, B/N개의 examples만을 선택하여 backward pass를 수행
- 이때 많이 겹쳐 있는 RoIs 사이에 correlated losses가 존재할 수 있으므로 NMS를 통해 높은 loss를 가지는 RoI를 기준으로 많이 겹치는 RoIs를 제거
- 이러한 OHEM은 foreground/background ration와 같은 hyperparameters를 사용하지 않음
> - 중요한 positive/negative examples가 backward pass에서 고려되지 않았다면 다음 iteration에서 loss가 매우 커지게 되고, 이는 해당 examples가 고려될 확률이 매우 높아진다는 것을 의미함
> - 또한, 특정 foreground RoI에 해당하는 example이 detection하기가 매우 쉽다면 모델은 자연스럽게 더 어려운 examples에 집중하도록 sampling하게 됨 
- OHEM은 학습 도중에 sampling 과정을 필요로 하지 않기 때문에 hard negative mining에 비해 매우 빠른 학습 속도를 보장함

## 실험결과
- OHEM을 (foreground ratio/background ratio, positive/negative ratio)를 사용하는 heuristic sampling 방법과 비교한 결과 heuristic을 사용할 때와 안할때 모두 OHEM의 성능이 훨씬 좋았음
- OHEM을 사용했을 때 미니 배치에 포함되는 이미지의 수 $N$을 줄여도 성능은 거의 변하지 않으며, 이는 OHEM의 robust함을 증명함
- 기존 Fast R-CNN을 훨씬 큰 배치 사이즈로 훈련하여 많은 RoIs가 고려될 수 있도록 하더라도 OHEM을 사용할 때보다 성능이 낮았음
- 또한, OHEM은 원활한 optimization을 돕는 효과도 있음
- OHEM을 사용하면 기존 Fast R-CNN보다 연산량과 메모리 소모량이 늘어나기는 하지만 수용할만한 수준임

## 의견
- / 