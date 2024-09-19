# Spatiotemporal Self-Supervised Learning for Point Clouds in the Wild (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Wu_Spatiotemporal_Self-Supervised_Learning_for_Point_Clouds_in_the_Wild_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/wu2023spatiotemporal.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 자율주행 차량의 발전에 따라 대규모의 unlabeled LiDAR 데이터가 생성됨
- 본 논문에서는 이러한 unlabeled data를 활용하기 위해 self-supervised learning을 사용함
> - 비교적 dense point clouds가 가능한 indoor 환경이 아닌 더 복잡하고 sparse한 outdoor 환경의 scenes에서 개발함
- 이전의 point cloud SSL은 contrastive learning을 진행할 때 같은 frame 내에서 augmentation을 적용하여 point clusters의 positive pairs를 생성함
> - 이는 LiDAR 데이터의 temporal한 정보를 고려하지 못함
- 본 논문에서는 spatial과 temporal domain을 모두 고려하여 positive pairs를 생성함
> - Negative pairs 없이 positive pairs만 사용함

## 접근법
- 우선 각각의 frame에 DBSCAN으로 point clustering을 수행
- 이후 point clusters 간의 distance를 비교하는 방식으로 연속된 프레임에서 point clusters를 매칭시킴 (Hungarian algorithm 사용)
- 각 clusters로부터 cluster-/point-wise features를 추출함
> - MinkUnet으로부터 모든 points에 대한 point-wise features 추출
> - Point-wise features를 cluster-level로 pooling하여 cluster-wise features 추출
- Point-wise feautres와 각 point가 속하는 cluster-wise features와의 L2 distance를 최소화하는 loss를 설계 (point-to-cluster loss)
- 연속된 frame에서 matching된 두 point clusters의 cluster-wise features 간의 L2 distance를 최소화하는 loss를 추가하여 temporal한 정보도 고려 (inter-frame loss)

## 실험결과
- KITTI와 nuScene을 pre-training 데이터셋으로 사용하고 SemanticKITTI와 manticPOSS를 downstream 데이터셋으로 사용함
- 다른 point cloud SSL과 비교했을 때 0.1%, 1%, 10%, 100%의 labeled data를 사용하는 semi-supervised/fine-tuning setup에서 가장 좋은 성능 기록
- Transfer learning setup에서의 성능도 가장 좋았음

## 의견
- /