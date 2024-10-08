# TransFG: A Transformer Architecture for Fine-Grained Recognition (AAAI, 2022)

[논문 링크](https://ojs.aaai.org/index.php/AAAI/article/view/19967)

<p align="center">
    <img width="600" alt='fig1' src="../img/he2022transfg.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Image Recognition (FGIR)은 subtle difference를 찾아야하므로 어려운 태스크
- 기존의 연구는 주로 CNN으로 진행됨
- 하지만 CNN은 locality property 간의 long-range relation을 파악하기 힘들며, FGIR에서 discriminative한 features들은 이미지의 특정 공간에 있는데 반해 CNN은 이미지의 전역적인 부분을 convolution하므로 이러한 정보들을 특정하기가 힘듦
- 반면 최근의 Transformer 구조는 초기의 처리 단계부터 locality property간의 관계를 파악하도록 훈련되며, 이미지를 각 패치 단위로 탐색하기 때문에 이미지 곳곳의 discriminative features를 특정하기도 유리함
- 따라서, 본 논문에서는 거의 처음으로 vision transformer (ViT) 구조를 FGIR에 사용함 (TransFG)
- Vision transformer의 특징에 따라 기본적인 성능이 좋았지만, 본 연구에서는 contrastive loss를 통해 모델의 분별력을 높여주는 Part Slection Module을 추가로 제안함

## 접근법
- 기존 ViT patch embedding과 달리 ovelapping patches로 나눔
- ViT 자체도 FGIR에서 뛰어나지만 discriminative한 성능 강화를 위해 Part Selection Module (PSM)을 도입함
- 모든 layer의 output activation을 곱하여 final activation map을 도출
- Final activation map의 각 attention head에서의 최대값을 계산하여 discriminative regions 선택에 사용함
- 도출된 index의 tokens와 classification tokens를 같이 마지막 transformer layer의 input sequence로 사용함
- 일반적인 분류 loss (cross-entropy) 외에도 discriminative regions의 특성을 더 잘 모델링 하기 위해 contrastive loss 사용
- 배치 내의 같은 class의 패치끼리는 가까워지도록, 다른 class의 패치끼리는 멀어지도록 학습

## 실험결과
- CUB-200-2011, Stanford Cars, Stanford Dogs, NABirds, iNat2017에서 평가함
- 몇몇 부분에서 SOTA 성능 달성
- Ablation study 통해서 제안하는 방법의 효과 입증
- 시각화를 통해 제안하는 방법의 효과 입증

## 의견
- ViT 기반 모델에도 제안하는 SSL 기법 적용할 수 있다는 걸 증명할 떄 사용 (일반 ViT, TransFG)