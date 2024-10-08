# Fast R-CNN (ICCV, 2015)

[논문 링크](https://openaccess.thecvf.com/content_iccv_2015/html/Girshick_Fast_R-CNN_ICCV_2015_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/girshick2015fast.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Object detection은 일반적인 classification과 달리 복잡하기 때문에 대부분의 detection 모델들은 여러 모듈로 구성되어 매우 느린 multi-stage pipeline을 사용함
- 이러한 모델의 복잡성은 (1) 우선 object의 위치를 특정하기 위해 매우 많은 potential region proposals를 추출해야 하고, (2) 이러한 proposals들을 개선하여 localization 정확도를 높여야하다는 점에서 기인함
- 본 연구에서는 주어진 proposals로부터 object를 분류하고 spatial locations을 refine하는 과정을 동시에 수행하도록 하여 detection 속도 및 성능을 개선하고자 함
- 이는 기존의 dection 모델들보다 학습 및 추론 속도가 매우 빠름
- 기존의 R-CNN은 다음과 같은 문제가 있었음
> - CNN, SVM, Bounding Box Regressor는 모두 따로 훈련됨
> - 2,000개 가량의 region proposal들이 각각 CNN에 입력되므로 메모리와 연산 속도에서 매우 비효율적
> - 결과적으로 detection 속도가 매우 느림
- 제안하는 방법은 R-CNN에 기반하고 있으며 기존의 R-CNN의 성능과 속도 모두 개선 (Fast R-CNN)

## 접근법
### (1) Overview
- Fast R-CNN은 입력 이미지와 region proposals를 입력으로 받음
- 입력 이미지는 CNN을 거쳐 피처맵으로 변환됨
- 피처맵은 ROI pooling layer를 거치고, 그 결과 각각의 region proposals에 대한 fixed-length feature vectors가 추출됨
- 추출된 각 피처들은 여러 fc layer를 거쳐 결과적으로 클래스 분류(K개의 object classes + background class)를 수행하는 softmax 레이어와 K개의 클래스마다 bounding box position을 refine하는 regression layer에 최종적으로 입력됨

### (2) ROI Pooling Layer
- R-CNN에서는 이미지에서 각각의 region proposals를 추출하여 CNN의 입력으로 사용했던 것과 달리, Fast R-CNN은 ROI pooling을 통해 하나의 이미지에서 모든 region proposals의 피처를 추출함
- 먼저 입력 이미지는 CNN (VGG-Net)을 거쳐 피처맵으로 변환됨
- 원본 이미지와 최종 피처맵의 subsampling 비율에 맞게 region proposals의 크기 및 중심 좌표를 조정하고 피처맵에 region proposals을 투영함
- 투영된 region proposal 영역은 고정된 크기의 sub-window로 나누어지고, sub-window로 나누어진 영역에 max pooling을 적용하여 해당 region proposal의 피처 벡터를 추출
- 미리 지정한 크기의 sub-window에서 max pooling을 수행하기 때문에 region proposal의 크기가 서로 달라도 고정된 크기의 피처 벡터를 추출할 수 있음

### (3) Initializing from Pre-trained Networks
- 사전 훈련된 VGG-Net이 backbone으로 사용됨
- VGG-Net의 마지막 max pooling layer는 ROI pooling layer로 교체
- VGG-Net의 최종 classification layer는 새로운 classification layer와 regression head로 변경
- 이미지와 region proposals를 동시에 입력으로 받도록 수정
- 추가로 Truncated SVD를 통해 fc layer의 사이즈를 줄이고 연산량을 줄임
### (4) Fine-tuning for Detection
- R-CNN 모델은 학습 시 region proposal이 서로 다른 이미지에서 추출되고, 이로 인해 학습 시 연산을 공유할 수 없다는 단점이 있음
- 따라서, 학습 시 feature sharing을 가능하게 하는 Hierarchical sampling 방법을 제안
- SGD mini-batch를 구성할 때 N개의 이미지를 sampling하고, 총 R개의 region proposal을 사용한다고 할 떼, 각 이미지로부터 R/N개의 region proposals를 sampling함
- 이를 통해 같은 이미지에서 추출된 region proposals끼리는 forward, backward propogation 시 연산과 메모리를 공유
> - 만약 N = 2, R = 128이라면 128개의 이미지에서 ROI를 하나씩 추출하여 훈련할 때보다 대략 64배 정도 빠름
- R/N개의 region proposal 중 25%는 정답 bounding box와 IOU가 0.5 이상인 positive samples로 구성하고, 나머지는 IOU가 0.1 이상 0.5 미만인 negative samples (background class)로 구성

### (5) Multi-task Loss
- Fast R-CNN은 한 번의 fine-tuning 훈련으로 feature extractor, classifier, regressor를 모두 훈련시킴
- Classification loss는 log loss 사용
- Regression loss outlier에 덜 민감한 smooth L1 loss를 사용
- Regression loss는 background class (negative samples)의 경우 적용되지 않으며 여러 box 예측 중 정답 class에 대한 box 예측만 적용됨

### (6) Fast R-CNN Detection
- 각 regions들의 결과 class probability를 기준으로 R-CNN과 마찬가지로 NMS를 적용하여 최종 결과 산출

## 실험결과
- Fast R-CNN 모델은 R-CNN 모델보다 학습 속도가 9배 이상 빠르며, detection 시 이미지 한 장당 0.3초(region proposals 추출 시간 포함)이 소요
- PASCAL VOC2007, 2010, 2021 모두에서 SOTA 성능 달성
- R-CNN, SPPnet과 비교했을 때 학습 및 추론 속도가 매우 빠름
- VGG-16 기반의 pre-trained 네트워크를 사용했을 때 유의미한 성능 개선 발견

## 의견
- /