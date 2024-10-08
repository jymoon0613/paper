# NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis (ECCV, 2020)

[논문 링크](https://arxiv.org/abs/2003.08934)

<p align="center">
    <img width="600" alt='fig1' src="../img/mildenhall2021nerf.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 novel view synthesis라는 vision task를 다룸 (객체를 찍은 여러장의 사진을 입력받아 새로운 view에서 바라본 객체의 모습을 예측)
- 본 논문에서는 5차원의 continous value를 입력으로 받아 해당 시점에서 바라봤을 때 객체에서 추출되는 radiance와 density를 추출하는 MLP를 훈련시킴 (Neural Radiance Field (NeRF))
- 이후 classical volume rendering techniques을 이용해 MLP로 얻은 RGB와 density 데이터들을 2D 이미지에 축적하여 새로운 관점에서 객체를 바라봤을 때 scene을 예측함
- NeRF는 미분가능하며, 따라서 정답 이미지와 예측된 이미지 간의 loss를 계산하여 모델을 최적화시킬 수 있음
- 즉, NeRF는 현실세계에 존재하는 복잡한 모양의 객체들을 표현할 수 있으며 이는 gradient-based optimization으로 학습 가능함
- 또한, 적은 용량만 있어도 3D모델의 scene을 생성할 수 있음

## 접근법
- NeRF는 입력데이터로 3D 좌표와, 바라보는 방향으로 구성된 5차원 입력을 받아 color와 volume density를 예측함
- 또한 NeRF가 특정 방향에서의 view만 잘 예측하는 것이 아니라, 어떠한 방향에서의 view인지 상관없이 잘 예측하도록 두 가지 설정을 함
> - NeRF가 3D 좌표만으로 volume density를 예측하도록함
> - NeRF가 3D 좌표와 바라보는 방향 정보를 모두 사용하여 RGB color를 예측하도록 함
- 이후 classical volume rendering techniques를 사용하여 렌더링을 수행
- NeRF의 학습 시 3D 좌표와, 바라보는 방향으로 구성된 5차원 입력을 바로 사용하게 되면 고주파 영역에서의 렌더링 성능이 떨어진다는 문제가 있었음
- 따라서, positional encoding을 적용하여 입력을 더 고차원의 데이터로 매핑해준 뒤 사용함
- Coarse network와 fine network로 모델을 분리하여 랜더링함으로써 랜더링 성능을 높이고 효율성을 개선함
- Coarse network와 fine network로 랜더링한 값과 실제값을 통해 total squared loss 계산

## 실험결과
- Novel view synthesis에서 좋은 성능 기록

## 의견
- /