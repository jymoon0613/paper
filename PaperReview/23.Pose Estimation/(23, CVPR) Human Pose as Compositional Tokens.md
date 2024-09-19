# Human Pose as Compositional Tokens (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Geng_Human_Pose_As_Compositional_Tokens_CVPR_2023_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/geng2023human.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Occlusion 상황에서의 정확한 human pose estimation은 아직까지 어려운 문제로 남아 있음
- 기존의 human pose estimation 시스템은 각 joint를 독립적으로 취급하여 joints 간의 상호 context 정보를 무시하며, 이것이 occlusion 상황에서의 비정상적인 예측을 유발함
- 따라서, 본 논문에서는 human pose를 compositional tokens로 모델링하는 새로운 기법을 제안함 (PCT)
> - 주어진 human pose를 $M$개의 token features로 인코딩함
> - $M$개의 token features는 codebook에 따라 quantize됨, 즉, 주어진 pose는 $M$개의 discrete indices로 표현됨
> - 이는 prototype poses, 즉, joints의 가능한 연결 조합을 미리 저장하는 것과 같음
- 실험 결과를 통해 PCT는 joints 간의 의존성을 모델링하는 데 뛰어나 occlusion이 발생하는 상황에서도 예측을 잘 수행함
> - Occlusion이 발생하더라도 visible한 joints에 따라 적절한 prototype pose를 선택하여 예측

## 접근법
- 우선 $D$ 차원의 $K$개의 joints를 compositional encoder로 입력하여 $M$개의 token features로 인코딩함
- 이후 learnable latent embedding space(codebook)을 정의하고, 각 token feature를 codebbok의 가장 가까운 entry index로 매핑함(quantization)
- Quantize tokens가 decoder로 입력되어 original pose(joints)가 복원됨
- L1 reconstruction loss를 최적화하는 방식으로 compositional encoder, decoder, codebook을 업데이트함
- 훈련된 codebook, decoder를 사용하여 human pose estimation을 token classification task로서 수행
> - Backbone network와 classification head는 입력 이미지로부터 token classes(indices)를 예측하고, compositional encoder로 추출한 ground-truth token classes와 cross-entropy를 계산하여 학습
> - Codebook에서 token indices에 따라 token features를 가져오고, decoder로 입력하여 pose를 예측 (예측된 pose와 ground-truth pose 간의 L1 reconstruction loss도 계산하여 학습)

## 실험결과
- COCO(2D), MPII(2D), H36M(3D) 데이터셋에서 SOTA 성능 달성
- CrowdPose, OCHuman, SyncOCC 데이터셋에서도 SOTA 성능 달성하여 심한 occlusion이 있는 상황에서도 강건한 예측이 가능하다는 것을 증명

## 의견
- /