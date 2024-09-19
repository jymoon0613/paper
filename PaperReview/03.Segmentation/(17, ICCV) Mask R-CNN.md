# Mask R-CNN (ICCV, 2017)

[논문 링크](https://openaccess.thecvf.com/content_iccv_2017/html/He_Mask_R-CNN_ICCV_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/he2017mask.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Faster R-CNN, Fully Convolutional Network (FCN) 같은 강력한 베이스라인의 존재는 object detection과 semantic segmentation에서의 빠른 성능 향상을 가져왔음
- Instance segmentation은 입력 이미지에서 모든 object를 검출해야 하고 각각의 instance를 정확하게 segment해야하므로 매우 어려운 task임 (object detection + semantic segmentation)
- 본 연구에서는 위의 베이스라인과 마찬가지로 빠른 학습 및 추론이 가능하면서 정확한 instance segmentation task를 위한 모델을 제시함 (Mask R-CNN)
- Mask R-CNN은 Faster R-CNN에 각각의 ROI에서 segmentation mask를 예측하도록하는 브랜치를 추가하여 확장한 것

## 접근법
- Faster R-CNN은 각 후보 ROI에 대해 두 브랜치를 통해 class label과 bounding box 조정값을 예측함
- Mask R-CNN은 여기에 하나의 브랜치를 더 추가하여 instance segmentation을 위한 object mask를 예측하도록 함
- Mask branch는 mxm 크기의 k개의 binary mask를 예측함 (k = class의 개수)
- Mask prediction에 대한 loss는 k개의 예측값 중 주어진 ROI의 ground-truth class label에 해당하는 mask prediction 결과에만 적용됨
- 이러한 방식은 class prediction과 mask prediction 과정을 분리하도록 함 (기존의 방법들은 하나의 output에서 여러 class에 대한 mask를 예측한 반면, Mask R-CNN에서는 class 별로 k개의 mask를 예측)
- Mask prediction은 이전의 class 및 bounding box 예측보다 spatial layout에 민감함
- 따라서, FC layer를 통해 작은 벡터로 축약시키게 되면 성능이 하락하기 때문에 mask branch는 FC layer를 사용하지 않고 일련의 convolution 연산으로만 구성됨
- 기존 Faster R-CNN의 ROI pooling은 CNN의 출력 피처맵에서 region proposals를 투영하고, 영역을 sub-window 기반으로 나누고 max pooling을 적용하여 고정된 크기의 피처로 추출하는 역할을 담당함
- 하지만 ROI Pooling의 이러한 quantization 과정은 ROI와 결과 feature vector 사이의 misalignment를 유발함 (크기를 맞추는 과정에서 (차원 고정을 위한 반올림 등) 일부 정보가 소실됨)
- Quantization은 실수(floating) 입력값을 정수와 같은 이산 수치(discrete value)으로 제한하는 방법
- 이러한 misalignment는 작은 변화에 invariance한 class prediction과 같은 경우에는 큰 영향이 없지만 pixel 단위의 정확한 결정이 필요한 mask prediction 같은 경우에는 큰 문제가 됨
- 따라서, ROI pooling으로 인한 misalignment를 제거하고 입력 ROI와 결과 feature vector 사이의 올바른 alignment를 수행하는 ROIAlign layer를 제안함
- ROIAlign layer는 기존의 ROI pooling layer에서 quantization 과정을 제거함
- 즉, 실수 차원 값에 대한 반올림을 진행하는 것이 아니라 bilinear interpolation을 통해 정확한 sampling point를 찾고 결과 벡터를 생성
- 이러한 ROIAlign layer는 ROI pooling layer와 달리 ROI와 feature vector 사이의 misalignment를 유발하지 않고 정확한 spatial location 정보를 보존
- ResNet+FPN을 backbone CNN으로 사용함

## 실험결과
- 당시 instance segmentation task의 SOTA 성능 기록

## 의견
- /