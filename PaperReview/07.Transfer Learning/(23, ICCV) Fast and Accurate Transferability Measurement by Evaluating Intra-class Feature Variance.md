# Fast and Accurate Transferability Measurement by Evaluating Intra-class Feature Variance (ICCV, 2023)

[논문링크](https://openaccess.thecvf.com/content/ICCV2023/html/Xu_Fast_and_Accurate_Transferability_Measurement_by_Evaluating_Intra-class_Feature_Variance_ICCV_2023_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/xu2023fast.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transfer learning은 머신러닝 분야에서 매우 중요한 컨셉이며 모델이 훈련하는 방식과 여러 도메인에 적용되는 방식을 변화시켰음
- Transfer learning은 특히 target domain의 labeled data가 매우 적은 상황에서 유용하게 활용됨
- Transfer learning에서 pre-trained 모델을 fine-tuning하는 것이 downstream task에서의 성능을 높이는 데 가장 쉽고 효과적인 방법임
- Transfer learning에서 가장 중요한 것은 여러 pre-trained 모델 set에서 가장 유용한 pre-trained 모델을 찾는 것
- Transferability measurement는 source task에서 훈련된 pre-trained 모델이 target task에 얼마나 transferable한지를 정량화하는 것
> - Pre-trained 모델을 주어진 task에 대해 순위를 매기는 용도로 사용
- Transferability measurement는 transfer learning 자체보다 빠르게 계산되어야 하고(fast) 다양한 시나리오에서 general하게 동작하며(general), 정확하게 최고의 pre-trained 모델을 찾아야 함(accurate)
- 본 논문에서는 간단하고 효율적인 transferability measurement method인 transferability measurement with intra-class feature variance(TMI)를 제안함
> - Transferability를 target task에 대한 pre-trained 모델의 일반화 수준으로 정의함
> - 일반화 수준을 정량화하기 위해 labeled target representation의 conditional entropy를 통해 intra-class variance를 측정함
> - Intra-class variance는 새로운 task에 대한 모델의 adaptability, 즉, 모델이 얼마나 transferable한지를 측정함
- TMI는 최고의 supervised 모델을 선정하는 용도뿐만 아니라, 최고의 self-supervised 모델 및 transferring layer를 선정하는 용도로도 사용 가능

## 접근법
- 본 논문의 목표는 transferability를 빠르고, 정확하고, general하게 측정하는 것
- 이는 다음과 같은 문제를 야기함
> - Speed: pre-trained 모델을 target task에 fine-tuning하는 과정은 큰 computational cost를 유발하므로 training 없이 transferability를 측정해야 함
> - Accuracy: 기존 방법들은 pre-trained 모델의 classifier만 fine-tuning하여 최적의 모델을 찾으므로 부정확하고, 정확하게 transferability를 측정하는 방법이 필요함
> - Generality: supervised pre-trained 모델뿐만 아니라 self-supervised pre-trained 모델 및 transferring layer를 찾는 데도 적용가능한 general한 방법이 필요함
- 본 논문에서는 다음과 같이 위 문제를 해결함
> - 한 번의 forward propagation을 통해 target representations의 compactness와 sparseness를 평가하여 transferability를 측정
> - Transferability를 target task에 대한 pre-trained 모델의 일반화 수준으로 보고, intra-class를 통해 generalization ability를 측정
> - Latent representations의 intra-class variance로 transferability를 측정하는 것은 여러 settings에서 general하게 적용될 수 있음
- Conditional entropy로 target representations에 대한 intra-class variance를 계산
> - Intra-class variance pre-trained 모델이 target tasks에 대한 generalization ability를 갖도록 함
- 우선 pre-trained 모델로부터 target task의 모든 labeled data에 대한 latent representations를 추출
- 이후 lantent representations로부터 intra-class variance를 계산(= transferability score)
- Transferability score가 가장 높은 pre-trained 모델을 선정하고 target task에 fine-tuning함
> - TMI가 클수록 transfer performance가 좋음

## 실험결과
- Supervised, self-supervised pre-trained 모델 그룹을 평가 대상으로 선정 (각각 50개, 11개)
- 다른 transferability measurement methods와 비교했을 때 TMI는 op-5 best source 모델, best source data을 가장 많이 찾았음
- Self-supervised 및 transferring layer를 찾는 데도 효과적

## 의견
- /