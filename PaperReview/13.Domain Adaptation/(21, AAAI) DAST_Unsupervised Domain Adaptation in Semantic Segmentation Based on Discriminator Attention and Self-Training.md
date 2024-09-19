# DAST: Unsupervised Domain Adaptation in Semantic Segmentation Based on Discriminator Attention and Self-Training (AAAI, 2021)

[논문링크](https://ojs.aaai.org/index.php/AAAI/article/view/17285)

<p align="center">
    <img width="600" alt='fig1' src="../img/yu2021dast.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Unsupervised domain adaptation(UDA)의 핵심 아이디어는 source와 target domains의 feature-level distributions을 매칭시키는 것
- 이러한 feature matching은 이미지의 서로 다른 regions에 따라 그 난이도가 다를 수 있음
> - 'Sky'에 대한 adaption의 경우, 어떤 domains든 상관없이 비슷한 특징을 보이므로 비교적 쉬움
> - 'Buildings', 'Traffic lights'의 경우 domain의 스타일 등에 따라 상이하게 나타날 수 있으므로 adaption이 어려움
- Features를 전체 이미지 수준으로 global하게 align하는 것은 오히려 성능을 약화시킬 수 있으며, 이에 이미지의 서로 다른 regions 별로 서로 다른 alignment weights을 할당하는 방법이 제안되었음
- 본 논문에서는 hard-adapted local features를 찾고, 그에 따라 feature maps를 reweight하는 discriminator attention(DA) 전략을 제안함
> - DA는 discovering과 correcting이라는 두 단계로 구성됨
- 또한 UDA 이후에도 source data에 편향된 decision boundary를 보정하기 위해, 추가로 self-training(ST) 기법을 사용함
> - Pseudo-labels를 이용하는 방식임
- 요약하자면 본 논문에서는 semantic segmentation을 위한 UDA로서 DA와 ST를 결합한 DAST를 제안함

## 접근법
- 전체 아키텍처는 segmentation network(segmenter $S$)와 두 개의 discriminator networks(discover $D$, corrector $C$)로 구성됨
- $S$는 fully-convolutional한 네트워크이며, 다시 feature extractor $E$와 abel predictor $P$로 나눌 수 있음
- $D$와 $C$도 CNN 기반의 classifiers이며 fully-convolutional output을 출력함
> - $D$와 $C$의 output이 서로 다른 domains, 서로 다른 이미지 regions의 local alignment를 측정하는 confidence scores가 됨
- 주어진 입력 source 이미지에 대해, $E$는 feature maps $f_s$를 출력하고, $P$는 pixel-level semantic segmentation maps $p_s$를 출력함
> - $p_s$와 ground-truth labels를 사용하여 segmentation loss를 계산함
> - 또한, $f_s$와 $p_s$는 각각 $D$와 $C$로 입력되어 feature-level ($f_s$), output-level ($p_s$) adversarial learning에 사용됨
- Target image도 $E$를 통과하여 $f_t$가 출력되고, $f_t$도 $D$로 입력됨
> - $D$는 adversarial loss를 통해 $f_s$, $f_t$의 eature distribution을 align함
> - 또한, $f_t$의 각 locations에서의 alignment confidence score를 나타내는 attention map $\alpha$을 생성함
> - $\alpha$는 $f_t$에 대한 $D$의 출력에 절댓값을 취한 것임
> - $\alpha$에서 높은 값을 가지면, $D$가 해당 locations를 기반으로 domain 예측을 신뢰했다는 것을 의미하며, 해당 locations에 대한 adaptation에 더 집중할 필요가 있음을 의미함
- $\alpha$로 $f_t$를 reweight하고, reweighted된 $\hat{f}_t$는 $P$로 입력되어 pixel-level prediction $p_t$로 변환됨
> - $\hat{f}_t$는 $f_t$에 비해 poorly-aligned regions에 더욱 집중하도록 변환되었음
- 이후 $C$는 $p_s$와 $p_t$ 간의 adversarial learning을 수행함
> - Poorly-aligned regions의 adaptation을 강화하기 위해 $C$의 adversarial loss도 $\alpha$로 reweight됨
- 또한, target domain에 대한 decision boundary를 최적화하기 위해 self-training을 적용함
> - $p_t$의 전체 pixels 중 probability 상위 $q$ 퍼센트(i.e., 50%)에 대해 pseudo labels를 생성하여 학습하고, 나머지 pixels는 gradient backpropagation 하지 않음

## 실험결과
- SYNTHIA, GTA5를 source data로 사용하고, Cityscapes를 target data로 사용하여 평가를 진행함
- VGG-16, ResNet-101 기반의 DeepLab을 $S$ 구조로 사용함
- DA는 다른 UDA 방법들에 비해 좋은 adaptation 성능을 보여주었음
- DAST(ST가 추가됨)를 사용했을 때 성능이 더욱 개선됨

## 의견
- / 