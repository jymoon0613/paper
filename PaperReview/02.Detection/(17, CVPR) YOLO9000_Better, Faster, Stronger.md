# YOLO9000: Better, Faster, Stronger (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Redmon_YOLO9000_Better_Faster_CVPR_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/redmon2017yolo9000.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝을 이용한 object detection은 속도와 정확도를 크게 향상시켜줬지만 탐지할 수 있는 객체의 수는 적다는 문제가 있는데, 이는 object detection에 사용되는 데이터셋은 classification과 같은 다른 vision tasks와는 다르게 구축하기 어렵기 때문임
- 이를 해결하기 위해 본 논문에는 classficiation 데이터셋을 object detection에 합쳐서 학습시키는 방법을 적용
- 또한, detection과 classification 데이터를 모두 이용해 학습시키는 joint training algorithm을 제안
- 제안하는 모델은 기존의 YOLO 베이스라인의 detection 성능을 개선하였으며, 제안하는 dataset combination method를 통해 9000개 이상의 서로 다른 objects를 탐지할 수 있음 (YOLOv2 or YOLO9000)

## 접근법
### (1) Better
- YOLO는 당시 Fast R-CNN과 같은 detector에 비해 recall 성능이 낮고, localization error도 더 많았음
- 따라서, YOLOv2는 recall과 localization을 개선하는 것을 목표로 함
- 이를 위해 네트워크의 스케일을 키우는 대신 이전의 연구들로부터 보고된 다양한 아이디어들을 YOLO 구조에 추가함
> - YOLO의 모든 convolution layers에 Batch Normalization 추가
> - 224x224의 이미지 크기로 사전 훈련된 classification network를 사용하는 YOLO와 달리, 448x448의 이미지 크기로 추가적인 fine-tuning을 거친 high-resolution classifier 사용
> - Bounding box 좌표를 직접 예측하는 기존의 YOLO와 달리 Faster R-CNN과 마찬가지로 anchor boxes를 사용하고 box offset(조정값)을 예측 
> - Anchor box의 사용은 모델의 mAP를 오히려 감소시켰지만 recall은 개선되었음
> - K-means 알고리즘을 사용하여 최적의 anchor box 조합을 선정
> - Bounding box 예측 시 예측값을 조절하여 초기 학습을 안정화시킴
> - 최종 feature map에 이전의 보다 큰 사이즈의 feature map을 더해줌으로써 fine-grained features에 대한 접근을 가능하게 함
> - YOLOv2의 학습 과정 동안 10개 batches 마다 입력 이미지의 크기를 바꿈

### (2) Faster
- 속도 개선을 위해 백본으로 기존의 VGG-16 대신 Darknet-19를 사용함
- Darknet-19를 224×224크기의 이미지로 160 epochs 동안 학습시킨 다음 448×448크기의 이미지로 10 epochs 동안 fine-tuning함 (high-resolution classifier)

### (3) Stronger
- Classification과 detection용 데이터를 모두 학습에 사용하는 방법 제안
- 학습 과정에서 입력이 detection의 데이터인 경우 기존 YOLOv2의 loss function에 맞춰 모든 걸 backpropagate함
- 반대로 classification 데이터를 입력으로 받았을 경우엔 구해진 loss에서 classification에 해당하는 것만 backpropagate함
- 이때 classification과 detection의 데이터셋의 class 차이를 해결하기 위해 class의 계층적 구조를 활용한 WordTree를 만들어서 사용함

## 실험결과
- 기존의 baseline인 YOLO를 압도
- 2-stage detector인 Faster R-CNN보다도 높은 mAP를 기록
- 9000개가 넘는 object에 대한 detection을 수행할 수 있음

## 의견
- /