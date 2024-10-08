# ArcFace: Additive Angular Margin Loss for Deep Face Recognition (CVPR, 2019)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2019/html/Deng_ArcFace_Additive_Angular_Margin_Loss_for_Deep_Face_Recognition_CVPR_2019_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/deng2019arcface.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 face recognition(FR) 모델의 discriminative power를 더 개선하고 훈련을 안정화 시킬 수 있는 additive angular margin loss(ArcFace)를 제안함

## 접근법
- Softmax loss에서 features와 weights를 normalize하여 예측값이 features와 weights 사이의 각도에만 의존하도록 함
- 이때 ArcFace에서는 features와 weights 사이에 angular margin $m$을 추가하여 intra-class compactness와 inter-class discrepancy를 향상시킴
> - 서로 다른 identities에 대한 decision boundary가 모호한 softmax loss에 비해 ArcFace loss는 인접한 identities 간의 확실한 gap을 만듦
- ArcFace와 유사한 SphereFace와 CosFace의 경우 nonlinear angular margines를 지니는 반면 ArcFace는 constant한 linear angular margin을 지님
> - Constanct한 linear angular margin이 모델 훈련 안정화에 있어서 중요함

## 실험결과
- 여러 FR tasks에서 모두 SOTA를 기록

## 의견
- /