# FlexiViT: One Model for All Patch Sizes (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2212.08013)

<p align="center">
    <img width="400" alt='fig1' src="../img/beyer2023flexivit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT이 CNN과 가장 크게 다른 점은 이미지를 non-overlapping patches로 나누어 처리하는 것
- 패치를 기반으로한 여러 연구가 있었지만 (i.e. patch dropping, specialized tokens) 패치 사이즈에 대해서는 많이 탐구되지 않았음
- 본 논문에서는 패치 사이즈의 조절이 모델 파라미터 조정 없이 간단하면서 효과적으로 모델의 계산/예측 성능을 조절할 수 있다는 것을 증명함
> - 패치 사이즈에 general한 하나의 모델을 학습
> - 기존의 ViT는 서로 다른 패치 사이즈를 사용하기 위해 처음부터 다시 학습해야 했음
- 즉, 본 논문은 추가 비용 없이 여러 패치 사이즈를 자유롭게 선택할 수 있으면서 기존 fixed-patch ViT와 비슷하거나 더 뛰어난 성능을 보여주는 모델인 FlexiViT를 제안함

## 접근법
- ViT에서 patch size에 의존적인 파라미터는 patch embedding 과정에서 사용되는 위한 weights(embedding weights)와 position embedding임
- 기존의 훈련된 ViT에 대해 embedding weights와 position embedding을 interpolation으로 resize한 뒤 여러 패치 사이즈로 변경해가며 실험한 결과, 해당 모델이 학습되었던 패치 사이즈를 제외한 다른 패치 사이즈에서의 성능이 매우 안좋았음
- 제안하는 FlexiViT도 기존 ViT와 같은 setting에서 훈련됨
- 단, FlexiViT는 각 훈련 step에서 사전 정의된 패치 사이즈 목록에서 uniformly random하게 선택된 패치 사이즈를 사용하는 방식으로 훈련됨
- 이를 위해 기존의 training code에서 두 가지 사항이 변경됨
> - 패치 사이즈가 결정되면, 이에 맟춰서 조정되는 embedding weights와 position embedding을 정의해야 함
> - 이는 각각 32x32, 7x7을 기본 크기로 하는 learnable parameter로 정의됨
> - 또한, 여러 패치 사이즈에 대해 딱 나누어 떨어질 수 있도록 240x240의 입력 이미지 resolution을 사용함
- Embedding resize를 위해서 기존 ViT에서 사용하였던 standard linear resize를 개선하여 pseudoinverse resize를 제안함
- 또한, knowledge distillation을 통해 성능을 더 끌어올림
> - Knowledge distillation은 일반 supervised training보다 최적화가 힘들다고 알려져 있으며, 이러한 문제의 간단한 해결책은 student를 teacher와 최대한 유사하게 initialize시키는 것임
> - 하지만, teacher는 대부분 student보다 큰 아키텍처를 사용하기 때문에 impractical함
> - 하지만, FlexiViT 강력한 ViT techer의 weights을 student ViT의 weights로서 쉽게 초기화할 수 있음
> - 선택된 teacher 모델에 대해 FlexiViT student는 teacher의 patch embedding weights을 resize하여 사용하고, teacher의 position embedding은 bilinearly resample하여 사용함
> - 주어진 이미지에 대한 teacher의 예측과, 랜덤 패치 사이즈를 사용한 student의 예측 사이의 KL-divergence를 loss로 사용함

## 실험결과
- 여러 downstream tasks에서 서로 다른 패치 사이즈를 사용할 때, 서로 다른 패치 사이즈로 훈련된 ViT를 사용하는 것과 단일 FlexViT를 사용할 때의 성능 차이가 거의 없었음
> - 이것은 서로 다른 패치 사이즈를 사용하기 위해 ViT를 새로 훈련시키는 것보다, 단일 FlexViT의 patch size를 변경하여 사용하는 것이 더 효과적일 수 있음을 의미함
- 서로 다른 크기의 패치 사이즈를 사용했을 때 각 layer의 output이 변하는지를 확인했을 때 중간 일부 layer를 제외한 모든 layer에서 매우 유사한 표현이 생성됨
> - 즉, 실제로 여러 패치 사이즈에 general한 모델이 학습됨
- FlexiViT는 resource efficient하고, memory/compute 복잡도가 적은 transfer learning을 가능하게 함
> - 즉, transfer 훈련 시 큰 패치 사이즈로 훈련시킨 뒤, 작은 패치 사이즈로 모델을 평가해도 좋은 성능을 보임
- FlexiViT는 사전 훈련 시(transfer 전) 패치 사이즈를 랜덤화하여 학습하지만, 고정된 패치 크기로 사전 훈련한 뒤 transfer 시 패치 사이즈를 랜덤화하여 학습하는 것도 어느정도 효과가 있었음

## 의견
- /