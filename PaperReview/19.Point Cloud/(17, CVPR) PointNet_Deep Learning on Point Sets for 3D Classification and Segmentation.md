# PointNet: Deep Learning on Point Sets for 3D Classification and Segmentation (CVPR, 2017)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Qi_PointNet_Deep_Learning_CVPR_2017_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/qi2017pointnet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Point cloud는 정해진 format이 없기 때문에 이전의 연구들은 이를 3D voxel grid로 바꾸거나 image들의 집합으로 바꾸고 난 후 딥러닝 모델에 입력함
> - 데이터의 부피가 불필요하게 커지고 데이터의 natural invariance를 훼손할 수 있는 artifacts를 유발할 수 있음
- 따라서, 본 논문은 단순한 점들의 집합인 point cloud를 바로 입력으로 사용하는 모델인 PointNet을 제안함
> - 이때 point cloud는 (i) 점들이 어떤 임의의 순서로 들어오더라도 그 특성이 변해서는 안된다는 permutaion invariance와, (ii) 어떤 transformation이 취해지더라도 point 간의 거리/방향은 그대로 유지되어야 한다는 rigid motion invariance의 성질을 지녀야 함
- PointNet은 points cloud를 입력으로 받아 class labels 혹은 point 별 segment/part labels를 출력함

## 접근법
- Point cloud는 3D points의 집합으로 표현됨
> - 각 point $P_i$는 $(x,y,z)$의 좌표 벡터임
- 이때 3D point data는 크게 세 가지의 특성을 지님
> - Unordered: $N$개의 points를 입력으로 받는 네트워크는 $N!$ 개의 입력 조합에 대해 invariant한 출력을 내놔야 함
> - Interaction among points: Neighborhood points 간에 연관성이 있기 때문에 local structure에서 neighbor points를 관찰해야 함
> - Invariance under transformation: 어떠한 rigid transformation(e.g., rotation, translating)이 적용되더라도 point set의 특징이 변화해서는 안됨
- 이때 point cloud 입력의 permutaion invariance를 보장하기 위해 symmetric function을 사용함
> - Symmetric function: 변수들의 위치(순서)가 바뀌어도 결과가 같은 함수
> - 본 논문에서는 symmetric function으로 max pooling을 사용함
> - 입력 point cloud로부터 각 point마다 MLP를 통해 feature를 생성하고, 생성된 feature에 column-wise 형식으로 max pooling을 적용함
> - 즉, 1024 개의 channel을 갖는 $N$개의 points에 대해($N\times{1024}$) max pooling을 적용하여 1024 차원의 벡터를 추출($1\times{1024}$)
> - MLP의 weight는 모든 point에서 공유됨
- 네트워크의 output은 input set의 global features임
> - Classification의 경우 MLP 혹은 SVM을 연결하여 분류 수행
- 하지만, segmentation의 경우 global knowledge와 함께 local knowledge도 필요함
- 따라서, global features를 intermediate features와 concat함
> - 즉, 각 point마다 생성된 local feature에 global feature를 연결함으로써 local/global 정보를 결합함
> - 이후, 다시 MLP를 거쳐 각 point에 대한 semantic categories를 예측함
- 또한, rigid motion invariance를 보장하기 위해 T-Net을 사용함
> - 기본 아이디어는 affine transform을 통해 point data를 canonical space에 projection하는 것
> - 구체적으로, point data를 canonical space로 보내기 위한 affine transformation matrix를 예측하고, 계산된 transformation matrix를 기존 point data에 곱함

## 실험결과
- 3D object classification task에서 raw point cloud를 입력으로 사용하더라도 좋은 성능을 기록함
> - PointNet은 다른 methods와 비교했을 때 매우 빠름
- Part/semantic segmentation task에서도 좋은 성능 기록

## 의견
- / 