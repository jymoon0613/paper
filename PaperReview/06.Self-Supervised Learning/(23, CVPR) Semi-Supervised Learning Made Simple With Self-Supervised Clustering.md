# Semi-Supervised Learning Made Simple With Self-Supervised Clustering (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Fini_Semi-Supervised_Learning_Made_Simple_With_Self-Supervised_Clustering_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/fini2023semi.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 clustering 기반의 SSL을 semi-supervised learning과 결합함
> - SwAV와 DINO를 semi-supervised하게 확장함

## 접근법
- 같은 이미지로부터 augmented된 두 views는 encoder network를 거쳐 embedding됨
- 이후 cluster centroids를 사용하여 두 embeddings를 logits로 변환함
- Logits에 softmax function을 적용하여 cluster assignments를 구함
- 이때 cross-entropy loss를 사용하여 두 cluster assingments가 같아지도록 훈련함
- Clustering 기반의 SSL이 semi-supervised learning에 적용될 때 생길 수 있는 문제는 feature space에서의 clusters가 class labels와 매칭되는 것을 보장할 수 없다는 것임
> - 실제 class 분포와 misalign될 수 있음
> - 이상적인 상황은 clusters가 실제 class centroids의 중심에 위치하는 것
- 이를 위해, 만약 real label을 사용할 수 있는 상황에서는 real label을 사용하는 cross-entropy를 사용하여 align을 수행
- 이러한 방법을 통해 어떤 clustering 기반의 SSL도 semi-supervised learner로 사용할 수 있음
- 본 논문에서는 SwAV와 DINO를 사용함

## 실험결과
- CIFAR 및 ImageNet에서 다른 semi-supervised methods보다 좋은 성능을 기록함

## 의견
- /