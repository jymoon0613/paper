# Inside-Outside Net: Detecting Objects in Context With Skip Pooling and Recurrent Neural Networks (CVPR, 2016)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Bell_Inside-Outside_Net_Detecting_CVPR_2016_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/bell2016inside.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 Fast R-CNN을 두 가지 측면에서 확장함
> - (i) fine-grained details를 더 잘 포착하기 위해 multi-scale representation을 활용함 (object의 inside information)
> - (ii) spatial Recurrent Neural Networks (RNN)을 사용하여 contextual information을 추출하고 활용함 (object의 outside information)
- 실험 결과에서, 제안하는 inside-outside network(ION)은 detection 성능을 크게 개선하였음
> - Multi-scale representation은 small objects에 대한 detection 정확도를 개선하는 효과가 있었음
> - Contextual information은 심하게 occluded된 objects에 대한 detection 정확도를 개선하는 효과가 있었음

## 접근법
- 먼저 single CNN은 입력 이미지로부터 feature maps를 출력함
- Backbone의 마지막 output feature maps는 2개의 4-directional spatial RNN을 통과하여 context feature maps를 추출함
> - 상하좌우 4방향에 대해 각각의 RNN을 정의함
> - Feature maps 상에서 주어진 방향으로 이동하며 recurrent 연산 수행
> - 4개의 spatial RNN의 output을 concat하여 출력함 (spatial dimension은 변하지 않음)
> - 두 번째 spatial RNN 연산을 마친 뒤 1x1 convolution을 수행하여 입력 feature maps와 channel dimension을 맞춰줌
> - RNN을 제외하고도 LSTM, GRU 등이 사용될 수 있음
- Context feature maps와 함께 backbone의 여러 intermediate feature maps로부터 주어진 proposals에 대한 고정된 크기의 features를 추출함 (multi-scale features)
> - Region proposals 생성을 위해 selective search와 같은 알고리즘 사용함
- 추출된 multi-scale features를 concat하여 최종 detection에 사용함

## 실험결과
- COCO, VOC2007, VOC2012 데이터셋에서 Faster R-CNN, FRCN, HyperNet 등의 다른 object detector보다 향상된 성능을 기록함

## 의견
- / 