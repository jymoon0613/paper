# DaDA: Distortion-aware Domain Adaptation for Unsupervised Semantic Segmentation (NIPS, 2022)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2022/hash/76931eaba1fcb55b70cde7d0de0161ef-Abstract-Conference.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/jang2022dada.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Segmentation task에서의 unsupervised domain adaptation(UDA)은 주로 생성된 이미지-실제 이미지, 서로 다른 도시 뷰 간에 발생하는 domain shift에 대해 다루고 있음
- 하지만, source와 target의 geometric/optical distortion으로 인한 domain shift에 대해서는 거의 다루지 않았음
- 본 논문에서는 직선의, 일반적인 도로주행 이미지를 source로 사용하고, geometric deformations이 큰 fisheye view 이미지를 target으로 사용하여 UDA를 수행함 (distortion-aware domain adaptation, DaDA)
- 먼저, source와 target domain 간의 상대적인 deformation을 모델링하는 relative distortion learning(RDL)을 제안함
> - 이를 위해 source 이미지를 target 이미지와 유사하게 변형시키는 deformation field generator를 디자인함

## 접근법
- Generator는 source 이미지와 target 이미지에 대해 diffeomorphic transformation을 적용하여 서로의 변형 스타일을 교환하도록 함
- 변환된 이미지를 역변환시켜 reconstruction loss를 계산하고, 변환된 이미지에 대한 segmentation adaptation loss를 계산하여 generator를 훈련시킴
> - Domain adversarial loss도 사용함
- Segmentation network는 adversarial segmentation loss와 entropy minimization loss를 통해 학습됨

## 실험결과
- 같은 source/target 조건에서 baseline UDA method에 DaDA를 추가했을 때 유의미한 성능 향상이 있었음

## 의견
- / 