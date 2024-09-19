# Contextual Priming and Feedback for Faster R-CNN (ECCV, 2016)

[논문링크](https://link.springer.com/chapter/10.1007/978-3-319-46448-0_20)

<p align="center">
    <img width="800" alt='fig1' src="../img/shrivastava2016contextual.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 2-stage detector에서 region proposal은 주로 image segmentation(i.e., selective search)와 같은 bottom-up 방식으로 구현됨
- 본 논문에서는 region proposals를 생성하기 위해 top-down의 context 정보를 이용하고자 함
> - Semantic segmentation을 모델의 sub-task로 사용하여 region proposal과 object detection의 feedback signals로 활용함
> - Semantic segmentation은 objects 간의 contextual relationships를 포착해야 하는 task이기 때문에 region proposal과 detection을 refine하기에 적합함
> - 또한, semantic segmentation을 통한 feedback signals는 feature extraction을 수행하는 backbone에도 전달되도록 함

## 접근법
- Faster R-CNN을 baseline을 사용함
- 본 논문은 semantic segmentation을 Faster R-CNN의 RPN과 Fast R-CNN(FRCN) module의 feedback signals로 사용함
- 먼저, semantic segmentation을 위해 ParseNet을 사용함
> - ParseNet과 Faster R-CNN을 동시에 정의함
> - 이때 정의된 ParseNet과 Faster R-CNN은 초기 10개의 convolutional layers를 공유하고 있음
- 정의된 프레임워크(Base-MT)는 multi-task learning의 형태로 ParseNet의 semantic segmentation 결과, Faster R-CNN의 detection 결과에 대해 동시에 학습됨
- 이때 semantic segmentation 결과로 Faster R-CNN을 가이드하기 위해 ParseNet의 output segmentation map을 RPN과 Fast R-CNN의 입력에 같이 append하여 사용함
> - 주이진 segmentation map을 통해 RPN과 Fast R-CNN은 background를 무시할 수 있고, occluded된 object, 매우 작은 object에 대한 정보도 획득할 수 있음
- 이러한 feedback을 backbone에도 적용시키기 위해 확장함
> - ParseNet의 output segmentation map과 원본 이미지를 concat하여 다시 shared backbone에 입력함
> - 이때 입력 layer 이후 각 convolutional layer의 입력은 이전 layer의 output feature maps에 segmentation map을 concat한 feature maps임
> - Shared backbone의 출력 feature maps를 사용하여 이전처럼 ParseNet과 Faster R-CNN의 예측을 수행함

## 실험결과
- PASCAL VOC, COCO에서 Fast R-CNN, Faster R-CNN보다 뛰어난 detection, segmentation 성능을 보여줌

## 의견
- / 