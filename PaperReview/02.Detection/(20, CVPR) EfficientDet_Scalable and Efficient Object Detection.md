# EfficientDet: Scalable and Efficient Object Detection (CVPR, 2020)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Tan_EfficientDet_Scalable_and_Efficient_Object_Detection_CVPR_2020_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/tan2020efficientdet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Detection 모델을 실세계의 환경에 적용하기 위해서는 model efficiency에 대한 고려가 필수적임
> - 이를 위한 one-stage detector, anchor-free detector 등의 해결책등이 제안되었지만, 이러한 방법들은 보통 낮은 detection 정확도를 보임
- 본 논문은 높은 정확도와 효율성을 동시에 고려하면서 scalable한 detection 아키텍처를 찾는 것을 목적으로 함
- 이를 위해 one-stage detector 패러다임에 기반하여 최적의 backbone, feature fusion, class/box network를 탐색함
- 이 과정에서 두 가지 문제점을 확인함
- 첫번째는 효율적인 multi-scale feature fusion 방법임
> - FPN은 대표적인 cross-scale feature fusion 방법 중 하나이지만 단순히 서로 다른 features를 더하는 방식으로 fusion을 진행함
> - 하지만 서로 다른 features는 서로 다른 scale로 존재하므로 fusion output에 기여하는 정도가 서로 다를 수 있음
> - 따라서, 본 논문에서는 서로 다른 features의 중요도를 학습하는 learnable weigths를 추가하고, top-down/bottom-up feature fusion을 반복적으로 수행하는 bi-directional feature pyramid network(BiFPN)을 제안함
- 두 번째는 model scaling임
> - 대부분의 이전 연구에서는 이미지 resolution을 키우거나 backbone network의 크기를 증가시켜 성능 향상을 이루었음
> - 하지만 본 논문에서는 feature network나 box/class prediction network를 scaling up하는 것도 성능과 효율성 측면에서 중요함을 확인함
> - 따라서 backbone, feature network, box/class prediction network의 resolution/depth/width을 동시에 scale up하는 compound scaling method를 제안함
- EfficientNet backbone에 BiFPN과 compound scaling method를 적용하여 EfficientDet 아키텍처를 디자인함

## 접근법
- 먼저 multi-scale feature fusion과 관련하여 FPN은 top-down 방식으로 multi-scale features를 fusion함
> - 상위 layer의 features에 convolution과 resizing을 적용한 뒤 직전 layer의 features에 더해주면서 fusion이 진행됨
- 이러한 top-down 방식의 FPN은 one-way information flow로 인한 성능 한계가 존재함
- 이를 해결하기 위해 PANet은 bottom-up path를 추가로 설계하였고 NAS-FPN은 architecture search를 통해 더 나은 cross-scale neural network topology를 찾음
> - 하지만, NAS-FPN은 최적의 구조 탐색을 위해 매우 많은 시간을 요구하며, PANet의 방법은 많은 추가 parameters와 연산량을 유발함
- 본 논문은 PANet의 구조를 일부 수행하여 BiFPN 구조를 제안함
> - (i) 먼저 FPN을 구성하는 최상단, 최하단에서의 convolution 연산을 제거함
> - 최상단은 상위 layer로부터 multi-scale features를 전달받지 않으며, 최하단에서는 multi-scale features를 더이상 밑으로 전달하지 않으므로 feature network에 대한 contribution이 다른 stage에 비해 상대적으로 낮음
> - (ii) 또한, bottom-up 과정에서 같은 level의 original features를 residual connection으로 연결해줌 (최상단, 최하단 제외)
> - 이는 추가적인 cost 없이 feature fusion을 향상시킬 수 있음
> - (iii) 마지막으로 birectional path를 한번만 사용하는 PANet과 달리 bidirectional path를 하나의 feature network layer로 보고 같은 layer를 여러 번 반복함
> - 이는 더 많은 high-level feature fusion을 가능하게 함
- 또한 각 multi-scale features의 중요도에 따라 단순합이 아닌 weigthed fusion을 진행함
> - Trainable scalar weight (w/ normalization)을 사용하는 방법, softmax가 적용된 weigth를 사용하는 방법, softmax를 사용하지 않고 weight 범위를 0-1로 제한하는 fast normalization을 사용하는 방법이 있음
> - Softmax 기반의 weigth과 거의 유사하지만 훨씬 빠른 fast normalized fusion을 사용함
- EfficientNet backbone에 BiFPN을 연결하여 EfficientDet 구조를 설계함
> - Backbone의 3-7 level features가 BiFPN에 연결됨
> - class/box network의 weigths는 모든 level features에서 공유됨
- Backbone, BiFPN network, class/box network, resolution을 동시에 scaling up하는 compound scaling method를 사용하여 EfficientDet model family를 구축함
> - Backbone은 EfficientNet의 width/depth coefficients를 그대로 사용함
> - BiFPN의 width/depth를 조절하는 scaling factor를 설정함
> - Box/class network의 width는 BiFPN과 항상 동일하며 depth에 대해서만 scaling factor를 정의함
> - Input resolution도 마찬가지로 scaling factor를 정의함

## 실험결과
- Detection 및 segmentation tasks에서 평가함
- Low-accuracy regime(accuracy보다 efficiency를 강조하는 경우)에서 EfficientDet는 훨씬 적은 파라미터와 연산량으로도 같은 목적의 다른 모델들과 비슷하거나 더 좋은 성능을 기록
- High-accuracy regime(efficiency보다 accuracy를 강조하는 경우)에서는 NAS-FPN을 비롯한 모델들의 성능을 뛰어넘음

## 의견
- / 