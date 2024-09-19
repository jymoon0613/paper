# Scalable Object Detection using Deep Neural Networks (CVPR, 2014)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2014/html/Erhan_Scalable_Object_Detection_2014_CVPR_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/erhan2014scalable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 object detector를 훈련시키는 한 방법으로 적은 수의 object candidate bboxes를 generage하는 방법을 제안함 (DeepMultiBox)
> - Bboxes는 single CNN으로 생성됨

## 접근법
- Box predictor는 AlexNet 구조로 되어 있으며, 주어진 입력에 대해 $i$번째 object box와 그 confidence score를 출력함
> - Object box는 last hidden layer에 linear transformation을 연결하여 네 개의 좌표로 표현됨
> - Confidence score는 last hidden layer에 sigmoid를 연결하여 0과 1사이의 실수로 표현됨
- Inference 시 100개 혹은 200개의 bbox가 예측됨
> - Confidence scores와 NMS를 적용하여 high-confidence bboxes만 추출
> - 각 bboxes에 대해 classification 수행
> - 이때 classify되는 bboxes의 수가 이전의 방법들보다 매우 적어지므로 더 강력한 classifier를 사용할 수 있게 됨
- Box predictor는 $M$개의 ground-truth objects로 구성된 이미지에 대해 $K$개의 bbox $ confidence score predictions를 생성함
- 이때의 훈련 목표는 예측 bboxes가 ground-truth bboxes와 최대한 잘 매칭되게 하는 것이며, 두 bboxes가 잘 매칭되는 경우 confidence score를 최대로, 그렇지 않은 경우 최소로 하는 것임
- 이를 위해 각 training example에 대한 assignment problem을 정의함
> - $j$번째 ground-truth bbox에 할당된 $i$번째 bbox prediction에 대해 두 bboxes 사이의 $L_2$ distance가 최소가 되도록 하는 matching loss를 정의함
> - 또한, 할당된 bbox에 대해서는 confidence score가 최대화되도록, 할당되지 않은 bbox에 대해서는 최소화도록 하는 confidence loss를 정의함
> - 최종 total loss는 matching loss와 confidence loss의 weighted sum으로 정의됨
- 각 training sample에서 total loss가 최소화되는 $i-j$ pair를 최적의 assignment로 설정함
- 생성된 각 object candidate bboxes에 대해 NMS를 적용하고, AlexNet 구조의 classifer가 class prediction을 수행함

## 실험결과
- Box predictor는 적은 bbox prediction만으로도 준수한 object detection rage(num. detected objects / num. bbox predictions)을 보여줌

## 의견
- / 