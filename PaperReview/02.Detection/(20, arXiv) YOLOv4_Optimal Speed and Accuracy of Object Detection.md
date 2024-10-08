# YOLOv4: Optimal Speed and Accuracy of Object Detection (arXiv, 2020)

[논문링크](https://arxiv.org/abs/2004.10934)

<p align="center">
    <img width="400" alt='fig1' src="../img/bochkovskiy2020yolov4.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 기존의 CNN 기반의 object detector는 정확한 탐지를 위해서는 느리게 동작하며, 빠르게 탐지하는 경우 정확도가 떨어진다는 문제가 있었음
- 따라서, 본 논문에서는 하나의 GPU만으로 매우 빠른 학습이 가능하면서 정확한 YOLOv4를 제안함

## 접근법
### (1) Related Work
- 최근의 object detector는 크게 두 부분으로 나뉘어짐
> - Feature 추출을 담당하는 pre-trained된 backbone
> - Class 및 bounding box를 예측하는 head
> - Head는 또 다시 one-stage detector와 two-stage detector로 분류될 수 있음
- 최근에는 backbone의 서로 다른 stage의 feature maps를 수집하는 neck이 backbone과 head 사이에 추가됨 (e.g. FPN)
- 또한, 모델의 inference 복잡도는 늘리지 않으면서 정확한 모델을 훈련시키기 위해 bag-of-freebies(BoF) 방법이 주로 사용됨
> - Training strategy만을 변경시켜 training에 대한 부담만을 가중시키는 방법 (e.g. focal loss, data augmentation)
- 반대로 inference 복잡도를 아주 조금만 증가시키면서 정확도를 크게 높여주는 추가 모듈 혹은 post-processing을 사용하는 방법인 bag-of-specials(BoS)가 사용되기도 함 (e.g. SPP, attention module, NMS)
### (2) Method
- 본 논문에서는 모델의 성능을 개선하기 위해 다양한 방법을 조합하여 최고의 성능을 보이는 모델인 YOLO v4을 제안함
- CSPDarknet53를 backbone으로 사용하며, SPP와 PAN을 neck으로 사용하고, YOLOv3의 head를 사용함
> - SPP(Spatial Pyramid Pooling): conv layer의 마지막 feature map을 고정된 크기의 grid로 분할한 후 평균을 구해 고정된 크기의 representation을 얻음
> - PAN(Path Aggregation Network): FPN의 top-down pathway에 bottom-up Path aggregation을 추가하여 low-level features의 정보를 high-level features에 효과적으로 전달함으로써 객체 탐지 시 localization 성능을 향상시킨 네트워크
- 여러 BoF 방법들이 사용됨
> - Bag of Freebies (BoF) for backbone: CutMix and Mosaic data augmentation, DropBlock regularization, Class label smoothing
> - Bag of Freebies (BoF) for detector: CIoU-loss, CmBN, DropBlock regularization, Mosaic data augmentation, Self-Adversarial Training, Eliminate grid sensitivity, Using multiple anchors for a single ground truth, Cosine annealing scheduler, Optimal hyperparameters, Random training shapes
- 마찬가지로 여러 BoS 방법들이 사용됨
> - Bag of Specials (BoS) for backbone: Mish activation, Cross-stage partial connections (CSP), Multiinput weighted residual connections (MiWRC)
> - Bag of Specials (BoS) for detector: Mish activation, SPP-block, SAM-block, PAN path-aggregation block, DIoU-NMS

## 실험결과
- YOLOv4를 다양한 object detector와 비교한 결과 속도 및 정확도 측면에서 가장 뛰어났음 (43.5% AP at MS COCO, 65 FPS at TeslaV100)

## 의견
- /