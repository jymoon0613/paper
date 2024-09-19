# FCOS: Fully Convolutional One-Stage Object Detection (ICCV, 2019)

[논문링크](https://openaccess.thecvf.com/content_ICCV_2019/html/Tian_FCOS_Fully_Convolutional_One-Stage_Object_Detection_ICCV_2019_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/tian2019fcos.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 대부분의 object detection networks는 anchor boxes를 사용함
- 하지만, 이는 (i) detection performance가 anchor box의 hyper parameters(i.e., aspect ratio, sizes)에 크게 좌우되고, (ii) 고정된 크기의 anchor box로 인해 매우 크거나 작은 object에 대응하기 힘들며, (iii) 좋은 recall rate를 달성하기 위해 anchor boxes를 입력 이미지 상에 dense하게 일일이 배치해야 하고, (iv) 각 anchor boxes에 대해 ground-truth box와의 많은 연산(i.e., IoU)를 계산해야 하므로 복잡하다는 문제가 있음
- 따라서, 본 논문에서는 anchor boxes를 사용하지 않는, one-stage 구조의, fully-convolutional한 아키텍처를 제안함 (FCOS)
> - Object detection을 semantic segmentation과 유사하게 per-pixel classification 관점에서 접근함
> - 즉, feature maps의 각 spatial location에서 bbox, class 예측을 수행함

## 접근법
- Convolutional feature maps의 각 position에서, class scores와 함께 해당 위치를 중심으로 주어진 bbox가 사방으로 얼마나 떨어져 있는지를 예측함
> - 이때 기준이 되는 position이 원본 이미지 상에서 ground-truth box 내에 위치하고 background class가 아닌 경우에 positive sample로 사용됨
> - 만약 해당 position이 여러 ground-truth bbox에 속해 있는 경우 ambiguous sample로 처리하며, 크기가 가장 작은 bbox를 target으로함
- Classification과 bbox regression은 backbone feature maps에 4개의 convolution layers를 추가로 연결하여 계산됨
- 또한, FCN 구조를 적용하여 multi-level feature maps에서 예측을 수행함
- 하지만 FOCS는 여전히 anchor box 기반의 detector보다 낮은 성능을 기록하였으며 저자들은 이러한 낮은 성능이 object center와 너무 멀리 떨어진 부정확한 bbox 예측 때문이라고 주장함
- 따라서, 이러한 low-quality bbox를 억제하기 위해 classification output 계산 시 추가로 center-ness를 계산하는 single branch를 추가함
> - Center-ness는 해당 spatial position에서 해당 position이 담당하고 있는 object의 중심까지의 normalized distance임
- 훈련 시 이러한 center-ness를 예측하도록 하는 loss function을 추가하고, inference에서는 class score에 예측된 center-ness를 곱해줌
> - 즉, center-ness는 object center와 멀리 떨어진 bbox의 scores를 줄이는 효과가 있음
> - 따라서 low-quality bbox는 NMS 과정에서 필터링될 가능성이 매우 높아짐

## 실험결과
- FPN을 사용했을 때 bbox에 대한 recall rate이 크게 증가하였으며, ambiguous samples의 수도 크게 즐어듦
- Center-ness의 추가는 성능을 크게 증가시킴
- Anchor-based detector(RetinaNet)보다 좋은 성능을 기록함
- YOLOv2, SSD 등 one-stage detectors와 비교했을 때 향상된 성능을 보여줌
- FCOS는 two-stage detector의 region proposal networks로도 사용가능함

## 의견
- / 