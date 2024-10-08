# AttentionNet: Aggregating Weak Directions for Accurate Object Detection (CVPR, 2015)

[논문링크](https://www.cv-foundation.org/openaccess/content_iccv_2015/html/Yoo_AttentionNet_Aggregating_Weak_ICCV_2015_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/yoo2015attentionnet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 object existence estimation과 bbox optimization을 single CNN 모델로 수행할 수 있는 직관적인 detection method를 제안함
- 이를 위해 기존의 CNN 구조를 기반으로 주어진 task에 적합하도록 AttentionNet을 디자인함
- AttentionNet은 bbox 예측을 위해 좌표를 직접 예측하는 regression 방식을 사용하는 것이 아니라 classification model의 weak predictions를 aggregate하여 exact bbox를 추정하는 방식을 사용함
> - 전체 이미지로부터 crop size를 조절하면서 반복적으로 cropping을 수행하며, 이때 cropping된 image part는 다시 AttentionNet으로 입력됨
> - Bbox(cropped image)가 target object에 fitting될 때까지 반복을 수행함
> - 이러한 접근법은 CNN이 강점을 보이는 classification 환경을 최대한 유지할 수 있음
- 이러한 AttentionNet의 특징으로 인해 object proposals 생성을 위한 별도의 모델이나 bbox regression 없이도 SOTA detection 성능을 달성할 수 있었음
- 하지만 AttentionNet은 이미지 내에 하나의 object만 존재하는 경우에만 활용될 수 있음

## 접근법
- AttentionNet은 VGGNet을 backbone을 사용함
- 입력 이미지에 대해 AttentionNet은 top-left(TL) corner와 bottom-right(BR)에 대한 directional predictions를 수행함
> - TL에 대한 predictions로는 'go to right', 'go to right-down', 'go to down', 'stop here', 'no instance in this image'가 있음
> - BR도 방향에 대한 정보만 반대가 되며 나머지는 TL과 똑같음
- Directional predictions를 바탕으로 image crop을 수행하며, cropped image는 다시 AttentionNet으로 입력되어 위의 과정을 반복함
- 종료 조건은 두 directional predictions가 'stop here'이거나 'no instance in this image'인 경우임
> - 두 directional predictions가 'stop here'인 경우는 입력 이미지 내의 object를 성공적으로 detect하여 detection process를 완료한 경우임
> - 두 directional predictions가 'no instance in this image'인 경우는 입력 이미지 내에 object가 존재하지 않아 detection을 수행할 수 없는 경우임
- 종료 조건에 도달하면 최종적으로 crop을 수행하고, cropped image에 대한 object classification을 수행함
- 제안하는 scheme으로 모델이 학습하기 위해서는 training samples를 일부 변경해야 함
- 따라서, ground-truth bbox를 기준으로 여러 bbox samples를 생성하고, 적절한 TL, BR labels를 할당해줌
> - 가능한 TL, BR의 조합은 positive samples의 경우 모든 가능한 directions에 대해 $4\times4=16$, negative samples의 경우 TL, BR 둘 다 'no instance in this image'인 경우 1가지임
- 학습 시, positive samples와 negative samples의 비율을 반반으로 설정함
- AttentionNet을 multiple objects가 존재하는 보다 general한 setting으로 확장하기 위한 방법도 제시함
> - 먼저 AttentionNet은 multi-scale, multi-aspect의 inputs를 입력 받음
> - 모든 예측값에 대해, TL이 'go to right-down', BR이 'go to left-up'인 경우에 대해서만 crop을 수행하고, cropped images를 하나의 새로운 이미지로서 위의 single-instance의 경우처럼 모두 처리해줌
> - 왜냐하면  TL이 'go to right-down', BR이 'go to left-up'인 경우 해당 region 안에 instance가 존재한다는 의미이기 때문임
> - 이는 AttentionNet의 directional predictions를 마치 region proposal로 사용하는 것과 같음
> - Detection 결과로 생성된 많은 bboxes에 대해 우선 bbox merge를 적용함
> - 이후 남아 있는 bboxes는 다시 크기를 일부 키운 뒤 AttentionNet에 입력하여 bbox optimization을 해줌
> - 마지막으로 다시 bbox merge를 수행하여 최종 예측값을 산출함

## 실험결과
- VOC human detection task, bottle detection task에서 SOTA 달성

## 의견
- / 