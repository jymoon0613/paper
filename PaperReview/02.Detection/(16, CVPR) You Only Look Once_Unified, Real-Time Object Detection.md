# You Only Look Once: Unified, Real-Time Object Detection (CVPR, 2016)

[논문 링크](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Redmon_You_Only_Look_CVPR_2016_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/redmon2016you.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 인간의 시각 시스템처럼 빠르고 정확한 object detection 알고리즘은 자율 주행과 같은 여러 어플리케이션에 적용 가능함 
- 이전까지의 object detection 시스템은 기존의 classification 모델의 변형을 통해 detection을 수행했음
- Deformable Parts Model (DPM)과 같은 시스템은 입력 이미지의 여러 위치와 스케일에서 object에 대한 classification을 진행하는 방식으로 동작함
- R-CNN은 region proposal을 사용하여 potential bounding box를 생성하고 각각을 classify하지만, 전처리, 사후처리 과정을 포함하면 이러한 방식은 매우 느리고 계산량이 많음 (또한, 모델을 구성하는 각각의 모듈은 모두 따로 훈련되어야 하므로 복잡)
- 본 연구에서는 object detection 문제를 입력 이미지에서, Bounding Box 좌표, 클래스 확률 등의 값을 한번에 예측하는 단일 regression 문제로 재구성함 
- 즉, 제안하는 모델을 사용하면 입력 이미지에 대한 한번의 end-to-end 처리만으로 object의 종류 및 위치를 파악할 수 있음 (You Only Look Once (YOLO))
- YOLO는 단일화된 구조를 지니며, 하나의 CNN 모델이 object detection에 필요한 모든 과정을 한 번에 처리함 
- 따라서, YOLO는 1) 비디오에 대한 실시간 처리가 가능할 만큼 매우 빠르고, 2) region proposal 혹은 sliding window를 사용하는 방식과 달리 예측 시 이미지의 전체적인 부분을 참고하므로 전체적인 이미지와 객체의 contextual information을 파악하기에 유리하고, 3) 객체의 일반화 가능한 좋은 표현을 학습하므로 다른 도메인 데이터셋으로의 이전이 쉬움 

## 접근법
### (1) Overview 
- YOLO는 전체 이미지로부터 추출한 피처를 사용하여 bounding box를 예측하며, 예측된 bounding box의 object 또한 동시에 예측함 
- 이러한 YOLO의 단일화되고 간단한 구조는 좋은 detection 성능을 보장하면서도 매우 빠른 예측을 가능하게 하며, end-to-end 학습이 가능하게 함 
- YOLO는 이미지를 SxS의 그리드 셀로 나누고, 객체의 중심점이 어느 그리드 셀에 포함되면, 해당 그리드 셀이 해당 객체를 포함하는 영역으로 처리됨 
- 각 그리드 셀은 B개의 bounding boxes와 각 bounding boxes의 confidence scores를 예측함 
- Confidence score는 해당 box에 객체가 있을 가능성이 얼마나 되는지에 대한 정보와 함께 예측한 box 좌표가 정확한지에 대한 정보가 반영되어 있음 
- 일반적으로 confidence score는 해당 box에 객체가 있을 가능성과 예측된 bounding box와 ground truth bounding box 사이의 곱으로 정의됨 ($Pr(Object) * IOU_{pred}^{truth}$)
- 만약 셀에 object가 존재하지 않는다면 confidence score는 0이 되고 (Pr(Object) = 0 이므로), object가 존재한다면 confidence score는 $IOU_{pred}^{truth}$가 됨 (Pr(Object) = 1 이므로)
- 각 bounding box에는 box 좌표와 관련된 (x, y, w, h) 및 confidence score로 구성된 5개의 예측값이 존재함 
- 또한, 각 그리드 셀 단위로 (즉, 하나의 그리드 셀에서 B개의 bounding boxes에 대해 공통으로) 특정 객체가 포함되었을 확률을 예측하는 데, 이는 confidence score에 대한 고려도 같이 이루어짐
> - $Pr(Class_i|Object) * Pr(Object) * IOU_{pred}^{truth}$
- 요약하자면, 만약 S=7, B=2, C(클래스 개수)=20라면 하나의 이미지에 대한 최종 예측은 $S*S*(B*5+C) = 7*7*(2*5+20) = 7*7*30$ 차원의 tensor가 됨

### (2) Network Design 
- Deep CNN 구조를 사용 (GoogLeNet의 영향을 받음) 
- 24개의 convolution layer 및 2개의 fc layer를 사용 
- GoogLeNet의 Inception Module 대신 1x1 convolution 및 3x3 convolution을 차례로 사용

### (3) Training
- Backbone 네트워크를 ImageNet 데이터셋으로 사전 훈련시킴 
- 사전 훈련된 네트워크에 약간의 convolution layer와 fc layer를 추가하여 object detection에 사용 
- 입력 사이즈로는 224 x 224 혹은 448 x 448이 사용됨 
- 5개의 예측값 및 정답값 (x, y, w, h, confidence score) 사이의 regression loss (sum of sqaured error)를 통해 loss를 계산함 
- 모델의 정상적인 수렴을 위해 bounding box 좌표에 대한 loss의 비중은 높이고, object가 없는 박스의 confidence score 예측에 대한 loss 비중은 감소시킴
- 또한, inference 시에는 각 그리드 셀마다 B개의 bounding box를 예측하지만, 훈련 시에는 각 셀마다 ground truth box와 IOU가 가장 높은 1개의 bounding box 예측만이 사용됨
- 최종적으로 YOLO는 bounding box 좌표, 클래스 예측을 포함한 multi-part loss function을 최적화하도록 훈련됨

### (4) Inference 
- 매우 빠르게 Inference 수행 

### (5) Limitation of YOLO 
- YOLO는 각 그리드 셀에서 두 개의 bounding box만 예측하고, 하나의 그리드 셀은 하나의 클래스만 가질 수 있기 때문에 bounding box 예측에 있어서 강력한 제약을 부과함 
- 즉, 모델은 세 떼와 같이 그룹으로 나타나는 작은 물체를 탐지하는 데 어려움이 있음 
- 또한, 모델은 bounding box에 대한 예측 또한 end-to-end로 학습하므로 비정상적이거나 새로운 bounding box의 모양에 대해서는 예측이 어려움 
- 마지막으로, YOLO의 loss는 상대적으로 IOU에 큰 영향을 주는 작은 bounding box와 상대적으로 영향이 작은 큰 bounding box를 같은 비중으로 고려하기 때문에 incorrect localization으로 인한 error가 쉽게 발생함

### (6) Comparison to Other Detection Systems 
- Deformable Parts Models (DPM)은 object detection을 위한 구성요소가 분리되어 있는 반면, YOLO는 단일 프레임워크로 수행함 
- R-CNN은 sliding window 방식 대신 region proposals 알고리즘을 사용하며, 성능을 높이기 위한 복잡하고 무거운 파이프라인을 가지고 있음 
- YOLO는 R-CNN과 일부 공통적인 측면이 있지만 그리드 셀에 공간적 제약이 있다는 점, 보다 적은 bounding box를 예측한다는 점, 마지막으로 모든 요소가 단일 요소로 구성된다는 점이 다름 
- Fast R-CNN과 Faster R-CNN이 R-CNN의 속도 개선을 위해 고안되었지만 여전히 real-time으로는 속도가 느림 
- Deep MultiBox 방법이 region proposals이 아닌 CNN 모델 내부적으로 ROI를 찾고자 고안되었지만, 해당 방법은 여전히 복잡하고 무거운 파이프라인으로 구성되어 있으며 하나의 클래스만 예측 가능하므로 일반화될 수 없음 
- OverFeat은 localization을 위한 네트워크를 object detection에도 적용하였지만, 여전히 각 기능이 분리되어 있었고, 여전히 예측을 위해 global context가 아닌 local context만 참고할 수 있었음 
- MultiGrasp가 YOLO와 가장 유사하게 입력 이미지를 그리드 셀로 나누어 처리하도록 고안되었지만, MultiGrasp는 입력 이미지에서 object가 포함된 하나의 영역을 찾는 것에 국한되어 있으며, object의 크기, 경계선, 위치에 대한 추정은 불가능한 매우 간단한 모델이었음 

## 실험결과
- Pascal VOC 2007 및 Pascal VOC 2012에서 검증함 
- YOLO의 더욱 간소화된 버전인 Fast YOLO는 가장 빠른 object detector일 뿐만 아니라, 실시간으로 동작 가능한 detector 중 가장 정확함 

## 의견
- /