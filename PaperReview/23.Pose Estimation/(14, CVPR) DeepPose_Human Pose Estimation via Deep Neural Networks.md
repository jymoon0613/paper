# DeepPose: Human Pose Estimation via Deep Neural Networks (CVPR, 2014)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2014/html/Toshev_DeepPose_Human_Pose_2014_CVPR_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/toshev2014deeppose.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Human pose estimation은 human joints를 localize하는 vision task임
- Human pose estimation은 joints 연결 형태의 복잡성, 작고 거의 보이지 않는 joints, 폐색 및 context 포착의 필요성 등의 문제로 인해 매우 challenging한 task임
- 대부분의 연구는 가능한 모든 joints의 모양을 잘 포착하기 위한 방법을 찾는 것에 집중하고 있음
- 대표적으로, part-based 모델은 각 joint part를 추론하는 local detectors를 사용하게 되는데, 이는 body parts의 극히 일부 관계성만을 모델링하므로 computational cost에 비해 표현력이 제한됨
- 전체 이미지 수준에서 joints를 추론하는 방법(holistic manner)도 제안되었지만 real-world problems에 적용되기에는 제한적이었음
- 본 논문에서는 holistic manner를 사용하되 deep neural network(DNN) 기반의 human pose estimation model을 제안함
> - DNN은 각 body joint의 전체 context를 포착할 수 있음
> - Feature representation, model topology, joints 간의 상호작용, local detectors를 명시적으로 정의할 필요가 없으므로 매우 간단함

## 접근법
- 데이터는 입력 이미지와 k개의 joints 좌표 벡터로 구성됨
- 각 joint vector는 bounding box 형식으로 normalize됨
- 본 논문에서는 convolutional DNN을 모델로 사용하며, 모델은 입력 이미지로부터 2k개의 joint 좌표를 예측 (L2 loss)
> - k개의 joints에 대해 각각 (x,y) 두 개의 좌표가 존재
- 정의된 convolutional DNN은 전체 이미지를 입력으로 받아 joint 좌표를 예측하므로 fully holistic함
> - 전체 context를 고려
- 이때 모델은 220x220의 resolution을 사용하게 되는데, 이러한 고정된 크기의 입력은 모델이 detail한 표현을 학습하는 것을 제한할 수 있음 (coarse scale pose properties)
> - 입력 resolution을 키우는 것은 너무 많은 computational cost 증가를 수반함
- 예측 정확도를 높이기 위해 cascade pose regressors를 훈련시키는 방안을 제안함
> - 기본 initial 예측을 수행함
> - 이후 몇 개의 추가적인 convolutional DNN을 연결하고, 각 convolutional DNN은 이전 stage에서 예측된 좌표와 현재 예측된 좌표의 차이를 예측하도록 훈련됨
> - 예측 결과가 다음 stage에 연속적으로 반영됨
> - 이때 예측된 좌표를 기준으로 cropped된 이미지가 다음 stage에서 사용됨

## 실험결과
- Frames Labeled In Cinema(FLIC), Leeds Sports Dataset(LSP) 데이터셋에서 검증
- 이전 방법들과 비교했을 때 SOTA 성능 달성
- Cascade regressor는 initial prediction만 했을 때보다 성능을 크게 증가시킴

## 의견
- /