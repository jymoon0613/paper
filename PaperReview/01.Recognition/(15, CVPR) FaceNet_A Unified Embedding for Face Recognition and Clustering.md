# FaceNet: A Unified Embedding for Face Recognition and Clustering (CVPR, 2015)

[논문링크](https://www.cv-foundation.org/openaccess/content_cvpr_2015/html/Schroff_FaceNet_A_Unified_2015_CVPR_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/schroff2015facenet.png?raw=true"></br>
    <em><font size=2>Model structure.</font></em>
</p>

## 연구목적
- 본 논문에서는 face verification, recognition, clustering을 위한 unified system을 제안함 (FaceNet)
- 얼굴 이미지 간의 euclidean embedding을 학습하는 모델을 설계
> - 같은 인물의 얼굴 이미지 embedding은 서로 가까워지도록, 다른 인물의 얼굴 이미지 embedding은 서로 멀어지도록 학습
- 이를 통해 face verification, recognition, clustering을 직관적으로 수행할 수 있음
> - Face verification은 입력 이미지 embedding과 target 이미지 embedding 간의 거리가 특정 threshold 보다 큰지/작은지를 검사하여 구현 가능
> - Face recognition은 입력 이미지 embedding에 대한 KNN 분류 문제로 볼 수 있음
> - Face clustering은 얼굴 이미지 embedding space에서 K-Means 등의 알고리즘을 적용하여 수행 가능

## 접근법
- ZF-Net 혹은 Inception을 백본으로 사용함
- 모델은 얼굴 이미지 embedding 기반의 triplet loss로 학습함
> - 임의의 한 얼굴 이미지(anchor)는 해당 이미지와 동일한 사람의 다른 얼굴 이미지(positive)와는 embedding 거리가 최소가 되도록, 그 외의 이미지(negative)와는 embedding 거리가 최대가 되도록 학습함
- 이때 모델의 빠른 수렴을 위해 hard-positive 및 hard-negative를 적절히 선택하는 것이 중요함
> - 학습 시 mini-batch에 있는 모든 positive samples를 사용하되 negative sample은 anchor와의 embedding 거리가 최대가 되는 sample에 대해서만 loss를 계산함

## 실험결과
- LFW, Youture Faces DB에서 모두 SOTA 달성
- Face clustering에서도 좋은 성능 기록

## 의견
- /