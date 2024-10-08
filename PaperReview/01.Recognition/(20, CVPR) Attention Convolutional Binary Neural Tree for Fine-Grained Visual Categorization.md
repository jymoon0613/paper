# Attention Convolutional Binary Neural Tree for Fine-Grained Visual Categorization (CVPR, 2020)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Ji_Attention_Convolutional_Binary_Neural_Tree_for_Fine-Grained_Visual_Categorization_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/ji2020attention.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Visual Categorization (FGVC)는 하위 클래스 내부의 변동은 크고 하위 클래스 간 변동은 작기 때문에 매우 어려움
- 최근 CNN 기반의 접근법이 local subtle parts를 찾기 위해 사용되었지만 단일 CNN으로 이러한 세밀한 특징들을 완전히 포착하는 것은 한계가 있음
- 반면 여러 딥 모델들을 배치하여 서로 다른 object regions를 포착하게 하는 것이 매우 효과적이었음
- 따라서, 본 논문에서는 여러 local parts를 학습하기 위한 convolution 및 attention 연산을 트리 구조로 수행하고 통합하는 Attention Convolutional Binary Neural Tree (ACNet) 구조를 제안함
- ACNet은 weakly-supervised한 방식 (part annotation X, only class lable)을 사용하며 coarse-to-fine의 계층적인 특징 학습을 가능하게 해줌
- 트리 구조의 각 브랜치는 서로 다른 local parts를 담당하며 최종 inference는 각 최종 노드에서의 분류 결과를 통합하여 결정됨

## 접근법
- Fully binary tree 구조 사용 (노드의 개수 = (2 ** 트리의 높이) -1) (엣지의 개수 = (2 ** 트리의 높이) - 2)
- 하지만 네트워크가 서로 다른 스케일의 피처를 학습하도록 분기 시 왼쪽 브랜치에서는 attention module 2개, 오른쪽 브랜치에서는 attention module을 1개 사용하는 asymmetrical한 구조를 사용함
- 백본으로는 사전 학습된 CNN 모델의 레이어를 사용함
- 입력 피처가 오른쪽 혹은 왼쪽 브랜치 중 어디로 가야할지를 결정하기 위해 branch routing module을 설계함
- Branch routing module은 convolution 및 pooling으로 구성되어 있으며 최종 sigmoid 값에 따라 분기 방향이 결정됨
- Attention transformer module이 네트워크가 discriminative한 피처를 학습하도록 하기 위해 사용됨
- Attention transformer module은 서로 다른 receptive field 크기로 dilated convolution을 수행하는 Atrous Spatial Pyramid Pooling (ASPP)와 Squeeze & Excitation (SE) 모듈로 구성됨
- 최종 예측은 마지막 노드에서의 클래스 확률에 해당 최종 노드로 도착하는 확률 (branch routing module의 결과 누적값)을 곱하여 모든 최종 노드의 결과를 합한 것
- 학습 시에는 최종 예측에 대한 loss와 함께 각 최종 노드마다의 분류 loss도 같이 학습

## 실험결과
- CUB-200-2011, Stanford Cars, Aircrafts에서 검증
- SOTA 성능 달성
- 시각화를 통해 각 leaf node가 서로 다른 부분을 detect한다는 것을 확인
- Branch routing은 실제로 네트워크가 서로 다른 시각적 부분에 집중하도록 함

## 의견
- 트리 구조 사용하여 분류 모델 만드는 것이 흥미로움
- Related Works 깔끔함 나중에 논문 쓸 때 참고하면 좋을 듯