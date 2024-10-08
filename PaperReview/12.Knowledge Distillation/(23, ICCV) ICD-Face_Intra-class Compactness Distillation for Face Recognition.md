# ICD-Face: Intra-class Compactness Distillation for Face Recognition (ICCV, 2023)

[논문링크](https://openaccess.thecvf.com/content/ICCV2023/html/Yu_ICD-Face_Intra-class_Compactness_Distillation_for_Face_Recognition_ICCV_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/yu2023icd.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Face recognition(FR)의 실제 활용성을 개선하기 위해 lightweight FR 모델을 개발할 필요가 있음
> - 본 논문에서는 Knowledge disitllation을 사용함
- FR은 주로 open-set setting에서 평가되기 때문에 student 모델이 feature embedding을 discrimate하는 능력을 향상시키는 것이 중요함
> - Open-set FR: 학습 데이터에는 없는 unknown face가 test phase에 등장할 수 있음
> - 따라서 open-set setting에서는 closed-set setting에서 사용되는 classifier의 확률 분포가 아닌 feature embedding 간의 유사도가 판단 기준으로 사용됨
- 일반적인 FR distillation 방법은 student가 teacher와 같은 embedding space를 공유하도록 feature consistency distiilation(FCD)를 사용함
- 하지만, 본 논문에서는 FCD가 작은 student의 embedding space를 align하지 못한다는 문제를 발견함
> - Negative pairs에 대한 similarity distributions는 유사하여 inter-class discrepancy는 잘 유지함
> - 하지만 positive pairs에 대한 similarity distributions는 매우 낮아 intra-class compactness가 크게 감소함
- 본 논문에서는 FCD와 intra-class compactness distillation(ICD)를 포함하는 새로운 FR distillation framework를 제안함(ICD-Face)

## 접근법
- 먼저 teacher를 large-dataset에서 pre-train함
- 이후 teacher와 student의 embedding space를 align하기 위해 FCD를 수행
> - 하나의 batch에서 teacher와 student가 각각 추출한 features에 대해 L2 distance를 최소화하도록 함
- 이후 intra-class compactness를 강화하기 위한 ICD를 수행함
- 우선 하나의 batch에서 충분한 positive pairs를 만들어낼 수 없기 때문에 feature bank를 활용함
> - Teacher와 student 모두 각각의 feature bank를 보유함
> - Feature bank는 training dataset의 모든 identities 수, 각 identity마다 보유할 수 있는 최대 features 수, features의 차원의 3D tensor로 구성됨
> - 매 batch마다 FCD loss를 계산한 후 각 features로 feature bank를 업데이트함
> - Ground-truth label에 따라 feature bank에 해당 identity의 feature를 저장하며, 최대 개수에 도달하는 경우 가장 오래전에 저장된 feature를 대체
> - 매 iteration마다 업데이트된 feature bank와 현재 batch에 속한 features를 대응하여 postivie pairs를 생성
- Teacher와 student 모두 각각의 positive pairs로 similarity distribution을 구하고 similarity distribution consistency loss를 계산
> - 두 분포 간의 KL-divergence를 최소화

## 실험결과
- IARPA Janus Benchmark (IJB-B, IJB-C), MegaFace에서 평가를 수행
- ResNet-50을 teacher로 MobileNet-v2를 student로 사용함
- 두 데이터셋 모두에서 기존 FR distillation보다 높은 성능 달성함

## 의견
- /