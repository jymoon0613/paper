# PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space (NIPS, 2017)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2017/hash/d8bf84be3800d12f74d8b05e9b89836f-Abstract.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/qi2017pointnet++.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- PointNet은 3D point cloud를 직접적으로 처리하는 deep learning 모델임
- PointNet은 각 point featrues를 aggregate하여 global point cloud structure를 모델링하지만 local structure는 포착할 수 없음
- CNN의 성공은 local structure를 모델링하는 것이 중요하다는 것을 보여줌
> - Multi-resolution hierarchy에 따라 local patterns를 global context로 추상화(abstract)시키는 방식은 특히 unseen cases로의 일반화 측면에서 뛰어남
- 따라서, 본 논문에서는 3D point cloud를 hiearchical한 방식으로 처리하는 neural network인 PointNet++를 제안함
> - 먼저 point set을 distance metric을 기준으로 여러 overlapping local regions로 분할함
> - CNN과 유사하게, local regions로부터 fine geometric structures를 포착하기 위한 local features를 추출함
> - 추출된 local features는 서로 그룹화되어 더 큰 단위로 묶이며, 이러한 과정을 통해 점점 higher-level features를 생성할 수 있음
- 이때 고려해야 할 사항은 (i) 어떻게 point set으로부터 local group을 분할할 것인가와 (ii) 어떻게 local feature learner를 통해 local point groups/features를 higher-level features로 추상화할 것인가임
> - PointNet++는 local feature learner로 PointNet을 사용함
> - Local point groups는 임의로 샘플링된 points를 중심 좌표로 하여, 일정 거리 내에 있는 neighborhood points를 묶는 방식으로 생성됨
> - 즉, PointNet++는 PointNet에 hierarchical structure를 추가하여 확장한 모델임

## 접근법
- PointNet++의 hierarchical structure는 몇 개의 set abstraction levels로 구현할 수 있음
> - 각 level에서 local point sets가 처리되고 더 상위의 set으로 추상화됨
- Set abstraction level은 세 개의 주요 layers로 구성됨: sampling layer, grouping layer, pointNet layer
- Sampling layer는 입력 points로부터 local regions의 중심 좌표를 담당할 points를 선택함
> - 주어진 $N$개의 입력 points에 대해 farthest point sampling(FPS)를 반복적으로 적용하여 $N^\prime$개의 points를 샘플링함
> - FPS를 사용하면 서로 가장 멀리 떨어진 points를 선택할 수 있으며, 랜덤 샘플링과 비교했을 때 모든 points가 적어도 하나의 local regions에 포함될 수 있도록 보장해줌
- Grouping layer는 선택된 중심 좌표 주변의 neighboring points를 식별하여 local regions를 생성함
> - Grouping layer는 $N$개의 points features와 $N^\prime$개의 중심 좌표를 입력으로 받음
> - Ball query를 적용하여 $N^\prime$개의 중심 좌표에 대해 해당 좌표와 가장 가까운 $K$개의 points를 식별함
- PointNet layer는 mini-PointNet을 사용하여 local region patterns를 feature vector로 인코딩함
> - $K$개의 points로 구성된 $N^\prime$개의 local point regions를 입력으로 받음
> - 이때 local region의 point-to-point relations를 효과적으로 모델링하기 위해 각 region에 속하는 $K$개의 points의 좌표는 해당 region의 중심 좌표를 기준으로 한 상대 좌표로 변환됨
> - $N^\prime\times{K}$의 입력은 PointNet을 거쳐 $N^\prime$개의 중심으로 추상화됨
- 대부분의 point cloud 데이터는 일정하지 않은 density로 분포하고 있음
> - 예를 들어, dense한 point cloud 데이터에서 학습된 네트워크는 sparse한 point cloud 데이터에 대해 잘 일반화되지 못함
- 본 논문에서는 이러한 문제를 해결하기 위해 density adaptive PointNet layer를 제안함
> - Density adaptive PointNet layer는 다양한 scale의 point cloud 데이터를 학습함
- 이를 구현할 수 있는 한 방법으로 multi-scale grouping(MSG)가 있음
> - Grouping 단계를 다양한 scale로 여러 번 적용하여 하나의 중심점에 대해 여러 scale의 point group을 생성함
> - 각 point group에서 각각 추출한 feature vector를 concat하여 multi-scale feature vector를 추출함
> - 이때 각 point group은 임의의 dropout ratio를 선택하여 그 비율에 맞게 random하게 dropout하여 각 point group마다 서로 다른 scale로 균일하지 않은 density를 가지게끔 변환함
> - 이러한 과정을 통해 다양한 sparsity와 서로 다른 uniformity를 가지는 point data를 얻을 수 있음
- 하지만 MSG는 각 중심점들과 그 이웃한 점들이 모두 PointNet을 거쳐야 하므로 연산량 차원에서 아주 비효율적이고 time-consuming함
- 따라서, 이를 보완한 multi-resolution grouping(MRG)를 제안함
> - MRG는 서로 다른 scale로 얻은 두 feature vector를 concat하여 multi-scale feature vector를 생성함
> - 이 때 첫 번째 feature vector는 local group에 해당하는 points 전체에 대해 PointNet layer를 거쳐서 얻고, 두 번째 feature vector는 local group에 대해 그보다 한 단계 아래의 sub-region에서 얻은 features를 종합하여 얻음
> - 만약 input으로 들어오는 point cloud의 density가 낮다면, 첫 번째 feature vector에 의해 전반적인 특징에 대한 정보를 추출할 수 있고, density가 높다면 sub-region에 대한 feature가 더 디테일한 특징 정보를 제공할 수 있음
> - 즉, 두 feature vector를 모두 이용하게 되면 여러 density의 point cloud에 대해서 모두 대응할 수 있으며, 계산량 측면에서도 효율적임

## 실험결과
- 2D/3D object classification 및 point set segmentation tasks에서 좋은 성능을 기록함

## 의견
- / 