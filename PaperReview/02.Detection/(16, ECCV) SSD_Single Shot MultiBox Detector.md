# SSD: Single Shot MultiBox Detector (ECCV, 2016)

[논문 링크](https://link.springer.com/chapter/10.1007/978-3-319-46448-0_2)

<p align="center">
    <img width="600" alt='fig1' src="../img/liu2016ssd.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- R-CNN 계열의 2-stage object detector는 탐지 정확도가 높다는 장점이 있지만 embedded system이나 real-time applications에 적용하기에는 연산량이 많고 속도가 느리다는 단점이 있음
- 이러한 느린 탐지 속도를 해결하기 위한 많은 시도들이 있었지만, 속도를 빠르게 할수록 정확도는 낮아지는 trade-off가 크게 존재했음
- 따라서, 본 논문에서는 1-stage 방식으로 빠른 탐지를 하되 정확도를 경쟁력있는 수준으로 유지시킬 수 있는 object detection 모델을 제안함 (Single Shot Detector, SSD)
- SSD는 당시의 가장 대표적인 1-stage detector였던 YOLO보다도 탐지 속도가 빠르면서, 더욱 정확함
- SSD는 class와 bounding box를 사전에 정의된 default bounding boxes를 기준으로 예측함 (Faster R-CNN의 anchor box와 유사)
- SSD는 다양한 scale의 feature map 상에서 detection을 수행하여 탐지 정확도를 높임
- SSD는 end-to-end로 학습 가능하며 작은 입력 이미지에 대해서도 높은 탐지 정확도를 보장함

## 접근법
### (1) Architecture
- SSD는 VGG-16을 백본으로 사용하고 detection을 위한 auxiliary network를 추가한 구조임
- Auxiliary network는 몇개의 convolutional layers로 이루어져 있음 (fc layer를 사용하지 않으므로 추가적인 속도 개선 가능)
- SSD는 convolutional layers를 거치면서 feature map의 크기가 점차 줄어들 때 이러한 feature map을 추출하여, 여러 scale의 feature map 상에서 detection을 수행함
- 단일 scale의 feature map 상에서만 detection을 수행하는 경우 다양한 크기의 객체를 포착하는 것이 어렵기 때문에(ex. YOLO), SSD는 multi-scale의 feature map 상에서 detection을 수행하고 탐지 정확도를 개선
- Faster R-CNN과 유사하게 각 피처맵 상의 모든 위치에서 default bounding boxes를 사용
- 모든 multi-scale feature map 상에서 (c+4)xkxmxn 개의 예측이 수행됨 (c: class의 개수, k: default boxes의 개수, mxn: feature map의 크기)

### (2) Training
- 먼저 ground truth box와 가장 큰 IOU(jaccard overlap)를 가지는 box와 IOU가 0.5 이상인 box는 모두 positive로 labeling함
- Ground truth box와 0.5 미만의 IOU을 가지는 box는 negative로 label함
- 모델의 loss function은 confidence loss와 localization loss의 합으로 구성
- Localization loss smooth L1 loss를 사용
- Confidence loss는 모든 class에 대한 loss를 softmax loss를 통해 계산
- 서로 다른 aspect ration에 따라 6개의 default boxes를 사용함
- 이때 feature map의 크기에 따라 default boxes의 scale을 조정하여 feature map의 크기가 작아질수록 더 큰 객체를 탐지할 수 있도록 함
- Inference 시에는 최종적으로 NMS 적용

## 실험결과
- SSD는 속도와 정확도 측면에서 기존의 대표적인 1-stage detectore인 YOLO를 능가함

## 의견
- SSD 모델은 다양한 scale의 feature map에 다양한 scale과 aspect ratio를 가진 default box를 생성하여 다양한 크기를 가진 객체를 탐지