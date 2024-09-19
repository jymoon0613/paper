# DeNet: Scalable Real-Time Object Detection With Directed Sparse Sampling (ICCV, 2017)

[논문링크](https://openaccess.thecvf.com/content_iccv_2017/html/Tychsen-Smith_DeNet_Scalable_Real-Time_ICCV_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/tychsen2017denet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Two-stage detectors(i.e., R-CNN)와 one-stage detectors(i.e., YOLO)의 가장 큰 차이점은 object regions를 어떻게 식별하고 처리하는가에 있음
> - Two-stage detectors는 특정 알고리즘(i.e., selective search)을 사용한 preprocessing을 통해 object regions를 일부 선별함 (sparse sampling)
> - One-stage detectors는 정의된 grid의 positions에서 가능한 모든 object regions를 평가함 (dense sampling)
> - 보통 dense sampling 방식이 더 빠르다는 장점이 있음
- 본 논문에서는 sparse sampling 방식의 훈련 용이성, scene adaptability, 높은 정확도라는 장점과 dense sampling 방식의 빠른 훈련 및 평가라는 장점을 결합하는 새로운 모델 디자인을 제안하고자 함 (DeNet)
> - Sparse하게 sampling된 bboxes의 objectness를 예측하여 유효한 bboxes의 수를 줄이는 방법을 제안함

## 접근법
- ResNet을 backbone으로 사용하는 Faster R-CNN을 베이스라인으로 사용함
- Sparse하게 sampling된 bboxes에 대해 주어진 bboxes가 object instance를 포함하고 있는 bbox일 확률(non-null probability)을 예측하는 corner detection 모델을 추가로 정의함
- Corner detection model은 detection results와 함께 end-to-end로 훈련됨

## 실험결과
- One-stage detectors만큼 빠르면서 two-stage detectors만큼 정확한 detection 성능을 보여줌

## 의견
- / 