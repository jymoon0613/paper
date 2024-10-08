# Simple Baselines for Human Pose Estimation and Tracking (ECCV, 2018)

[논문링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Bin_Xiao_Simple_Baselines_for_ECCV_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/xiao2018simple.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Human pose estimation은 딥러닝의 발전과 함께 활발히 연구되었으며 그에 따라 네트워크 아키텍처 및 실험 방법도 점차 복잡해짐
> - 알고리즘 분석 및 비교가 더 어려워짐
- 따라서, 본 논문에서는 pose estimation 및 tracking을 위한 simple baseline을 제안함

## 접근법
### (1) Pose Estimation
- ResNet을 backbone으로 사용하며, ResNet의 마지막 conv. layer에 몇 개의 deconvolutional layers를 추가함
> - 3개의 deconv. layers (+ batch norm., ReLU)
- 네트워크의 끝에는 K개의 key points를 예측하기 위한 1x1 conv. layer를 부착함
- MSE를 사용하여 predicted heatmaps와 targeted heatmaps 간의 loss를 계산함
> - K번째 key point에 대한 targeted heatmap은 k번째 key point의 ground truth location을 기준으로 2D gaussian을 적용하여 만듦

### (2) Pose Tracking
- Multi-person pose tracking in video는 우선 주어진 프레임에서 human pose를 추정하고 고유한 id를 부여하여, 연속되는 프레임에서 계속 tracking하는 task를 의미함
- ICCV'17 PoseTrack Challenge에서는 Mask R-CNN으로 주어진 프레임의 human pose를 추정하고, greedy bipartite matching algorithm을 사용하여 online tracking을 수행하는 방식이 제안되었음
- 본 논문은 위의 pipeline을 따르되 두 가지 변경을 추가함
> - (1) 현재 프레임에서 human dectector가 찾은 box와 이전 프레임에서 optical flow로 찾은 box 두 가지 서로 다른 human boxes를 사용함
> - Human detector만을 사용하면 연속적인 프레임에서 motion blur 혹은 occlusion으로 인해 missing detection이 발생할 확률이 큼
> - 따라서, 이전 프레임으로부터 optical flow의 temporal information을 사용하여 boxes를 생성하여 이러한 문제를 보완함
> - (2) Greedy matching algorithm에 사용하는 similarity metric을 flow-based pose similarity metric으로 변경함
> - 기존에는 IoU를 similarity metric으로 사용하였지만, 이는 instance가 빠르게 움직이거나 프레임에 instance가 많이 존재할 경우 부정확할 수 있음
> - 따라서, body joints 간의 distance를 Object Keypoint Similarity(OKS)로 measure하는 flow-based pose similarity metric을 사용함

## 실험결과
- 훈련 시에는 이미지에서 고정된 크기의 aspect ratio로 ground-truth human box를 crop한 뒤 네트워크로 입력함
- Inference 시에는 two-stage top-down 방식이 적용됨
> - Faster R-CNN으로 human detection을 먼저 수행
> - Detect된 human boudning box 각각을 네트워크로 입력하여 single person pose estimation 수행
- CPN, Hourglass 모델과 비교한 결과 OHKM loss를 사용하지 않는 경우 제안하는 simple 모델의 성능이 더 높았음
- PoseTrack 데이터셋으로 pose tracking 성능도 평가하였으며 준수한 성능을 기록함

## 의견
- /