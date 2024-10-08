# Fine-Grained Visual Classification via Progressive Multi-Granularity Training of Jigsaw Patches (ECCV, 2020)

[논문 링크](https://link.springer.com/chapter/10.1007/978-3-030-58565-5_10)

<p align="center">
    <img width="600" alt='fig1' src="../img/du2020fine.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Visual Classification (FGVC)는 sub-categories에 속하는 object 간 차이가 매우 미세하기 때문에 일반적인 분류 문제보다 어려움
- 이를 해결하기 위해 명시적으로 local parts를 detect하거나, 암묵적으로 saliency localization을 수행하는 방법이 제안되어 왔음
- 이러한 연구는 대부분 manual part annotations에 기반하여 학습되는데, 이는 label 수집이 어렵다는 문제와 함께 error-prone하다는 문제가 있음
- 따라서, 최근에는 part annotations 없이 category label 만을 사용하는 weakly-supervised한 방법들이 제안되기도 했음
- 이러한 방법들이 주목한 점은 정확한 분류를 위해 가능한 많은 semantic local parts를 detect하는 것임
- 하지만, 이러한 방법들은 (1) 찾은 part 정보 중 어떠한 정보가 더 중요한지, (2) 찾은 part 정보들을 어떻게 결합하여 사용할 것인지에 대한 논의가 부족함
- 본 논문에서는 semantic local parts를 찾는 것도 중요하지만, 찾은 정보들을 어떻게 효과적으로 융합할 것인지가 더 중요하다고 주장함
- 따라서, 본 논문에서는 fine-grained parts를 학습하고 parts 간의 feature fusion을 같이 수행하는 통합된 프레임워크를 제안함 (Progressive Mylti-Granularity (PMG))
- 이는 (1) 서로 다른 세분화 수준에서 피처를 fusion하도록 하는 progressive training strategy와 (2) 네트워크가 특정한 세분화 수준의 피처를 학습하도록 하는 random jigsaw patch generation을 통해 수행됨
- Progressive training strategy를 통해 네트워크는 일정한 단계에 따라 학습하며, 각 단계마다 정해진 수준의 세분화 피처를 학습함 (4 stage)
- 네트워크는 가장 세분화된 정보로부터 점차 전체 이미지 정보로 시각을 확장함 (zoom-out)
- 각 stage에서 학습이 된 모델의 가중치는 다음 stage 모델의 학습 초기값이 됨
- 각 네트워크의 학습 정보는 최종 레이어에서 fusion됨
- 또한 각 stage에서 모두 같은 세분화 피처가 추출되는 것을 막기 위해 jigsaw puzzle generator를 사용함
- 즉, stage 4의 네트워크를 제외한 네트워크들의 입력으로는 jigsaw puzzling이 적용된 이미지가 입력으로 사용됨
  
## 접근법
- Progressive Learning 구현을 위해 3개의 스테이지에서 각각 피처맵 추출
- 추출된 피처맵은 벡터로 projection 후 분류에 사용
- stage1, stage2, stage3, concat 4개의 피처에 대해 각각 분류함
- 한 번의 이미지 배치에서, (1) stage1에 해당하는 layer까지 연산하고 피처 추출 후 분류 결과로 모델 학습, (2) stage2에 해당하는 layer까지 연산하고 피처 추출 후 분류 결과로 모델 학습, (3) stage3에 해당하는 layer까지 연산하고 피처 추출 후 분류 결과로 모델 학습, (4) 모든 피처 concat하고 분류 결과로 모델 학습
- progressive learning은 local features를 점진적으로 학습할 수 있다는 점에서 한번에 모두 학습하는 일반적인 방식과 차이가 있음
- 각 스테이지에서 모델이 서로 다른 세분화 특징을 학습하도록 jigsaw puzzling을 사용
- 효과 극대화를 위해 패치 사이즈는 해당 스테이지의 receptive field보다 작아야 하며 스테이지가 진행될수록 점차 커져야 함
- Inference에는 concat만을 사용하거나 concat+stage1만 사용함

## 실험결과
- CUB-200-2011, Stanford Cars, Aircrafts 데이터셋에서 검증
- 당시의 SOTA 성능을 개선
- 스테이지 수와 jigsaw puzzle의 patch size에 대한 ablation도 진행

## 의견
- jigsaw puzzle이 FGVC에서 매우 좋은 것 같음 (DCL에서도 사용했음)