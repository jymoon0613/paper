# Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks (NIPS, 2015)

[논문 링크](https://proceedings.neurips.cc/paper/2015/hash/14bfa6bb14875e45bba028a21ed38046-Abstract.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/ren2016faster.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Region proposal methods와 region-based CNN은 object detection의 성능 향상에 큰 기여를 함
- Fast R-CNN은 초기의 R-CNN의 속도를 크게 개선시켰지만 region proposals를 찾는 과정은 여전히 bottleneck으로 남아있음
- Region-based CNN은 GPU 연산을 통해 속도 이점을 누리고 있지만 region proposal method는 여전히 CPU에서 동작하므로 GPU 상에서 동작할 수 있는 region proposal method가 필요함
- 따라서, 본 연구에서는 detection network와 같은 파라미터를 공유하면서 GPU 상에서 빠르게 region proposals를 추출하는 Region Proposal Networks (RPN)을 도입하여 이러한 문제를 해결하고자 함
- 기존의 Selective Search 과정을 제거하고, region proposals를 찾는 과정을 네트워크 내부로 이전함으로써 Faster R-CNN은 입력 이미지에 대해 end-to-end로 동작하는 fully convolutional network가 됨

## 접근법
### (1) Region Proposal Networks (RPN)
- RPN은 입력 이미지에 대해 다수의 bounding box 좌표 및 objectness score를 출력함 (objectness score: 해당 box에 object가 존재할 확률 (1: 존재, 0: 배경(존재X)))
- Backbone으로는 ZF-Net 혹은 VGG-Net을 사용함
- 우선 입력 이미지에 대해 CNN backbone을 거쳐 피처맵 추출
- 피처맵에 3x3 convolution을 적용 (출력 피처맵의 채널 수 = 256 or 512, padding을 통해 resolution은 유지)
- 최종 피처맵이 각각 classifier와 regressor로 입력되어 region proposals 별 objectness score와 box 좌표가 계산됨
- Objectness score와 box 좌표는 모두 1x1 convolution으로 계산됨
- 최종 피처맵의 모든 위치에서 각각 k개의 region proposals를 추출함
- 이때 k개의 region proposals는 사전에 정의된 k개의 reference boxes인 anchors에 의해 추출됨
- 본 논문에서는 3개의 서로 다른 스케일과 3개의 서로 다른 aspect ratio로 구성된 총 9개의 anchors를 사용함 
- 즉, 피처맵의 모든 위치에 각각 9개의 anchors가 존재 (anchors의 총 개수 = HxWx9)
- 따라서, classifier의 출력은 HxWx9X2 = HxWx18 차원의 텐서이고 regressor의 출력은 HxWx9x4 = HxWx36 차원의 텐서

### (2) Loss Function for Learning Region Proposals
- 먼저 각 anchor에 binary class label (ground-truth objectness score)을 부여함
- Ground-truth bounding box와의 IOU가 0.7 이상인 경우 1 (positive samples)가 되며 0.3 이하인 경우 0 (negative samples)이 됨 (나머지 anchor는 학습 과정에서 무시)
- 학습은 모든 anchor에 대해, objectness score의 classification loss와 bounding box regression loss를 동시에 최적화하도록 훈련됨 (regression loss는 grounb-truth objectness label이 1일때 (positive anchor)만 계산)
- Fast R-CNN과 마찬가지로 classification loss는 log loss, regression loss는 smooth L1 loss
- 이미지 경계를 넘어가는 anchors는 loss 계산하지 않음

### (3) Optimization
- 하나의 이미지에서 256개의 anchors만 랜덤하게 추출함 (positive와 negative 비율 1:1)

### (4) Sharing Convolutional Features for Region Proposal and Object Detection
- RPN이 region proposals를 찾아내면 이후의 detection 과정은 Fast R-CNN과 일치함
- RPN과 Faster R-CNN이 같은 parameter를 share하고 end-to-end로 훈련시키기 위해 4-Step Alternating Training method를 사용
- 먼저 ImageNet에서 사전 훈련된 가중치를 사용하여 RPN만 end-to-end로 훈련시킴
- 다음으로 Fast R-CNN도 ImageNet에서 사전 훈련된 가중치를 사용하여 end-to-end로 훈련시키되, freeze된 RPN의 region proposals를 입력으로 사용함
- 세 번째 단계로, 백본 네트워크의 파라미터는 고정하고 RPN만 다시 튜닝함 (파라미터 공유 시작)
- 마지막으로, 역시 백본 네트워크의 파라미터는 고정하고 Fast R-CNN만 다시 튜닝함

### (5) Implementation Details
- 학습 시에는 이미지 경계를 넘어가는 anchors는 고려하지 않지만, inference 시의 이미지 경계를 넘어가는 proposals는 이미지 경계에 맞게 clip해서 사용
- 추출된 proposals의 redundancy를 감소시키기 위해 objectness scores를 기준으로 NMS를 적용
- NMS 이후 objectness scores를 기준으로 top-N 개의 proposals만 detection에 사용함

## 실험결과
- PASCAL VOC 2012 데이터셋에서 mAP 값이 75.9를 보이면서 Fast R-CNN 모델보다 더 높은 detection 성능을 보임
- Fast R-CNN 모델이 0.5fps(frame pre second)인 반면 Faster R-CNN 모델은 17fps를 보이며, 이미지 처리 속도 면에서 발전한 결과를 보임
- 또한 feature extraction에 사용하는 convolutional layer의 feature를 공유하면서 end-to-end로 학습시키는 것이 가능해짐

## 의견
- 속도가 많이 개선되었지만 아직 real-time까지는 버거움