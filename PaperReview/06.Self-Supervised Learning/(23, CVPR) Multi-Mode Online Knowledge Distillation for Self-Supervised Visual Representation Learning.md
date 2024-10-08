 # Multi-Mode Online Knowledge Distillation for Self-Supervised Visual Representation Learning (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Song_Multi-Mode_Online_Knowledge_Distillation_for_Self-Supervised_Visual_Representation_Learning_CVPR_2023_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/song2023multi.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Knowledge distillation(KD)을 self-supervised learning(SSL)과 결합하여 small model 성능을 boost하기 위해 사용하기도 함
> - Large teacher를 SSL로 학습시킨 뒤 small student의 KD 수행
> - 하지만, KD 과정에서 고정된 teacher를 사용하므로 teacher는 student model의 knowledge로 업데이트 될 수 없다는 문제가 있음
- 본 논문에서는 collaborative한 방식으로 teacher와 student를 동시에 학습하는 multi-mode online knowledge distillation method (MOKD)를 제안함
> - MOKD는 self-distillation mode와 cross-ditillation mode로 구성
> - Self-distillation mode에서 두 모델은 SSL로 독립적으로 훈련됨
> - Cross-distillation mode에서 두 모델은 서로의 knowledges를 교환함

## 접근법
- Self-distillation은 DINO를 사용
> - Model1과 Model2 모두 online encoder과 target encoder로 구성
- 이때 encoder로부터 나온 representations가 MLP 기반의 projection head(M-head)와 Transformer 기반의 projection head(T-head)로 각각 입력됨
> - 두 head 구현 방식에서 동시에 self-distillation 수행
- Cross-distillation은 기본적으로 두 모델의 target encoder를 교환하여 contrastive learning을 수행하는 것임
> - Model1의 online representations과 Model2의 target representations과의 contrastive loss 계산
- 이때 T-head의 cross-attention 활용
> - Online representations에 GAP를 적용한 뒤 target representations와 concat하여 Transformer에 입력
> - Transformer output과 online representations와의 contrastive learning 수행

## 실험결과
- MOKD로 훈련된 두 모델은 각각의 모델을 DINO로 훈련시켰을 때보다 성능이 더 뛰어났음
- DINO, MoCov3를 포함한 SSL과 성능 비교를 했을 때도 MOKD의 성능이 더 뛰어났음
- 다른 SSL-KD 방법과 비교했을 때도 성능이 가장 뛰어났음
- Transfer learning 성능도 좋았음

## 의견
- /