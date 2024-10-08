# Rich Feature Hierarchies for Accurate Object Detection and Semantic Segmentation (CVPR, 2014)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2014/html/Girshick_Rich_Feature_Hierarchies_2014_CVPR_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/girshick2014rich.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 2012년 CNN을 기반으로 한 AlexNet이 ImageNet Challenge에서 뛰어난 성능을 기록함 
- 이후, CNN 구조가 또 다른 CV task인 object detection 과제에도 적용될 수 있는지에 대한 논의가 진행됨 
- Object detection의 특징은 multiple objects가 존재하는 이미지에 대해 각각의 objects를 localizing하고 분류해야 한다는 것
- 논문의 저자는 CNN을 object detection 과제에 적용할 때 두 가지 문제를 해결해야 한다고 주장함
> - (1) 어떻게 이미지에 존재하는 다수의 객체를 localizing 할 것인가에 대한 문제 
> - (2) 깊은 CNN 구조를 학습시키기에 데이터가 부족하다는 문제
- 일반적인 classification과 달리 object detection은 object의 위치를 localizing해야 한다는 특징이 있음
- 본 연구에서는 'recognition using regions' 방식으로 localization 문제를 해결함
- 즉, 본 연구에서는 입력 이미지에 대해 우선 class와 무관한 2000개 가량의 potential regions를 추출하고 각각에 대해 CNN으로 피처를 추출함
- 추출된 피처는 SVM의 입력으로 사용되어 최종적으로 분류가 진행됨
- 이러한 구조는 Region proposal과 CNN을 결합한 것이기 때문에 Regions with CNN features (R-CNN)으로 명명함 
- 또한, 본 논문에서 사용하는 PASCAL VOC 데이터셋은 ImageNet에 비해 클래스의 수나 이미지의 종류, 개수가 빈약함 
- 본 연구에서는 CNN을 우선 ImageNet에서 supervised하게 pre-training 시키고 VOC 데이터셋으로 fine-tuning하는 방식으로 데이터 부족 문제를 해결함
- 즉, 본 논문은 앞서 언급한 두 문제를 해결하면서 CNN이 detection 과제에서도 성능을 크게 향상시킬 수 있다는 것을 증명하고자 함 

## 접근법
### (1) Overview
- R-CNN은 총 세 단계로 구성되어 있음 
- 입력 이미지에서 (1) 객체가 있다고 추정되는 영역들을 추출하고 (region proposals), (2) CNN으로 피처를 추출하고, (3) SVM으로 분류하는 단계로 구성 
- R-CNN은 기존의 CNN 구성에 Region Proposals 과정이 추가된 것으로 볼 수 있고, 이름도 이와 관련되어 있음 

### (2) Region Proposal
- 먼저, 주어진 이미지에서 객체가 있을 법한 영역들을 뽑아내야 함 = Region Proposals 
- R-CNN은 Selective Search 알고리즘을 사용하여 region proposals 추출
- Selective Search는 세 단계로 구성되어 있음. 
> - 색상, 질감, 영역크기 등 을 이용해 non-object-based segmentation을 수행하여 많은 small segmented areas들을 생성
> - 그 후, 크기, 질감, 색상 등을 바탕으로 유사도를 계산하고, Bottom-up 방식으로 small segmented areas들을 유사성 기준으로 합쳐서 더 큰 segmented areas들을 만듦 
> - 작업을 반복하여 최종적으로 N개의 region proposal을 생성 (약 2,000개)

### (3) CNN
- 만들어진 후보 영역들을 CNN의 인풋으로 투입 
- 후보 영역들은 약 2,000개 
- CNN 구조는 2014년 당시 ImageNet 챌린지에서 우승한 VGGNet의 구조를 사용 
- 하지만, detection을 위한 PASCAL VOC 데이터는 부족하다는 문제가 있었음 
- 이를 해결하기 위해 사전훈련 방법을 사용 
- VGGNet을 ImageNet 데이터셋으로 사전훈련하고, fc 레이어만 최종 목적인 PASCAL VOC 데이터셋으로 튜닝함 
- 이는 결국 detection도 이미지 분류의 개념이 포함되어 있기 때문에 CNN 구조는 재사용함으로써 적은 데이터로도 높은 성능을 낼 수 있다는 것을 의미 
- CNN의 입력은 227X227로 조정됨 
- 아웃풋으로 4096 차원의 피처 벡터 생성 

### (4) SVM
- 모든 region proposals에서 추출된 특징 벡터를 바탕으로 SVM을 이용하여 클래스 예측 
- 소프트 맥스를 쓰지 않은 이유는 당시 SVM이 이미지 분류에서 여전히 큰 인기를 끌고 있기도 했고, 저자도 소프트 맥스보다 SVM을 썼을 때 성능이 더 좋았다고 함 
- SVM은 각 클래스마다 모두 생성함 
- 즉, 하나의 region proposal에 대해 최종적으로 클래스 개수만큼의 class score가 추출됨 (최종 출력 차원 = N(region proposals의 수) * C(클래스 수))
- 모든 region proposals의 output class score를 통해 클래스 별로 non-maximum suppression (NMS)를 적용함

### (5) 학습
- 우선 backbone CNN은 ImageNet 데이터로 end-to-end pre-training됨
- 다음으로 backbone의 classification head는 VOC 데이터셋의 classification head (21개)로 교체됨
- 입력 region proposal과 ground truth box의 IOU가 0.5 이상이면 positive sample로 사용하여 예측 class와 정답 class 간의 classification loss 계산
- 반면 IOU가 0.5 미만이면 negative sample이 되어 정답 class는 background가 되고 해당 region proposal은 background class를 예측하도록 classification loss 계산
- SVM 훈련 시에는 positive sample은 이미지의 ground truth box가 되며 negative sample은 ground truth box와의 IOU가 0.3 미만인 region proposal을 사용함 (나머지는 무시)

## 실험결과
- 기존의 방식들보다 detection 성능을 크게 증가시킴 
- Selective Search의 결과로 만들어진 bounding box는 부정확할 수 있으므로 예측좌표와 원래좌표를 바탕으로 선형회귀를 진행하는 bounding box regression을 사용하여 bounding box를 재조정함으로써 성능을 개선시킴

## 의견
- R-CNN은 이미지에서 객체가 존재할만한 영역들을 찾아내고, 각 영역에 대해 피처를 추출하여, 분류 및 bounding box regression을 진행함
- 하지만 약 2,000개의 영역들이 모두 CNN의 입력으로 사용되고, Selective Search는 CPU 상에서 작동하므로 속도가 매우 느리다는 단점이 있음
- 또한, CNN과 분류 및 회귀 과정이 서로 분리되어 있으므로 backpropagation을 사용할 수 없고 end-to-end 학습이 불가능하다는 단점이 있음
- 따라서, 이후에 이러한 문제를 해결하고 성능을 더욱 개선한 Fast R-CNN, Faster R-CNN 등이 등장함