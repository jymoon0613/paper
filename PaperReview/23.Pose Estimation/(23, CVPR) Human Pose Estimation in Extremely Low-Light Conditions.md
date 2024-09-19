# Human Pose Estimation in Extremely Low-Light Conditions (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Lee_Human_Pose_Estimation_in_Extremely_Low-Light_Conditions_CVPR_2023_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/lee2023human.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 extremely low-light condition에서의 pose estimation task를 다룸
- 해당 task에서는 데이터 수집의 어려움과 정확한 pose estimation의 어려움이 있음
- 본 논문에서는 우선 extremely low-light image 데이터셋을 구축함
- 또한, extremely low-light pose estimation을 위한 learning using privileged information(LUPI) 기법을 제안함

## 접근법
- 우선 extremely low-light human pose estimation(ExLPose) 데이터셋을 수집함
> - 데이터셋의 각 entry는 하나의 하나의 이미지에 대한 low-light 버전과 well-lit 버전 pair 및 pose label로 구성됨
> - Low-light images의 경우 여러 brightness levels에서 수집
> - 251개 scenes에 대한 2556개의 low-light/well-lit pairs로 구성
> - Bounding box 정보와 14개의 body joints를 manual하게 annotate함
- ExLPose 태스크를 위한 LUPI 기법을 제안
- LUPI 기법은 well-lit images를 teacher로, low-light images를 student로 사용하여 학습을 진행
- 이때 teacher와 student를 통합한 단일 아키텍처를 디자인함
> - 이를 위해 입력 이미지의 lighting-condition에 따라 batch normalization(BN)을 분기하도록 함 (lighting-condition specific BN, LSBN)
> - LSBN을 제외한 나머지 모델 파라미터는 공유함
- LSBN은 low-light/well-lit에 대한 각각의 affine transform 파라미터로 구성된 두 개의 BN으로 구성됨
> - LSBN은 입력 배치에서 low-light/well-lit images를 식별하여 다른 BN을 수행
- Teacher와 student는 각각 well-lit images, low-light images를 사용하여 pose estimation loss로 학습함
- 또한, teacher와 student의 intermediate feature maps가 유사해지도록 supervision을 진행

## 실험결과
- DANN, AdvEnt와 같은 domain adaptation 기법과 비교했을 때 LUPI가 low-light 환경에서 성능이 가장 좋았음

## 의견
- /