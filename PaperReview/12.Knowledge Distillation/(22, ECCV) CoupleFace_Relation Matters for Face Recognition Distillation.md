# CoupleFace: Relation Matters for Face Recognition Distillation (ECCV, 2022)

[논문링크](https://arxiv.org/abs/2204.05502)

<p align="center">
    <img width="400" alt='fig1' src="../img/liu2022coupleface.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Face Recogninition(FR) 모델 경량화를 위해 knowledge distillation이 사용될 수 있음
- FR distillation의 가장 기본적인 방법은 student가 teacher의 embedding space를 모방하도록 하는 것
> - Teacher와 student embeddings 간의 L2 distance를 최소화 (feature consistency distillation, FCD)
- FCD를 사용하는 경우 student 모델의 inter-class discrepancy가 teacher에 비해 충분히 크지 않거나 intra-class compactness가 충분히 작지 않은 경우가 발생함
- 따라서, 본 논문에서는 teacher와 student의 mutual information을 사용하여 teacher와 student 간의 간극을 좁히는 mutual relation distillation(MRD)를 제안함
- 결론적으로, FCD와 MRD를 모두 사용하는 FR distillation method인 CoupleFace를 제안함

## 접근법
- 먼저 teacher는 large-scale FR dataset에서 pre-train되고, student와 FCD를 수행
- 이후 MRD를 수행
> - MRD는 teacher mutual relation(TMR) $R(f^{t}_{i}, f^{t}_{j})$를 활용하여 student mutual relation(SMR) $R(f^{s}_{i}, f^{t}_{j})$을 distill함
- 우선 teacher 모델로 dataset의 모든 features를 추출함
- 이후 각 identity 별로 identity prototype을 계산함
> - 각 identity에 속하는 features의 normalized mean
- 이후 서로 다른 identity prototype 간의 cosine similarity를 모두 계산하고, 각 identity 별로 최대 similarities를 갖는 top-$K$개의 prototypes를 선정하여 informative prototype set을 찾음
- 이후 각 입력 sample이 속하는 identity에 대해 TMR과 SMR을 구할 수 있음
> - SMR = $\{f^{s}_{i}, g^{k}_{i}\}^{K}_{k=1}$, TMR = $\{f^{t}_{i}, g^{k}_{i}\}^{K}_{k=1}$, $k\in\{1,\dots,K\}$, $i\in\{1,\dots,M\}$, $M$은 identity의 수
- 구한 SMR과 TMR로 relation-aware distillation(RAD) loss를 계산함
> - SMR과 TMR의 cosine similarity 차이(L1 distance)

## 실험결과
- IJB-B, IJB-C, MegaFace에서 평가함
- ResNet-50을 teacher로, MobileNetV2를 student로 사용함
- 세 데이터셋에서 모두 다른 FR distillation methods보다 성능이 좋았음
> - 훈련된 MobileNetV2는 ArcFace로 훈련된 ResNet-50과 거의 비슷한 성능 기록
- FCD와 비교했을 때 CoupleFace의 positive pairs에 대한 similarity distributions가 더 compact했으며, negative pairs에 대한 similarity distributions가 더 separable했음

## 의견
- /