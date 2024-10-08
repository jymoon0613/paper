# What to Hide from Your Students: Attention-Guided Masked Image Modeling (ECCV, 2022)

[논문링크](https://arxiv.org/abs/2203.12719)

<p align="center">
    <img width="500" alt='fig1' src="../img/kakogeorgiou2022hide.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MIM은 image domain에서 인상적인 성능을 기록하였지만 masking strategy에 대한 연구는 아직 많이 진행되지 않음
- MIM에서는 MLM과 마찬가지로 random한 image patches가 마스킹됨
- 하지만, image data에서 random masking은 효과적이지 않음
> - MLM에서 random word masking은 전체적인 semantic을 묘사하는 high-level concepts를 숨기는 것과 같음
> - 반면, MIM에서 image patches는 매우 redundant하기 때문에 random patch masking이 이미지의 interesting parts를 숨기는 효과가 있다고 할 수 없음 
- 따라서, 본 논문에서는 random masking의 한계를 극복하기 위해 self-attention map에 기반하여 더 discriminative한 regions를 마스킹하는 attention-guided masking(AttMask)을 제안함
> - MST와 유사하지만 MST는 low-attended patches를 마스킹하지만, AttMask는 highly-attended patches를 마스킹한다는 점에서 다름

## 접근법
- BYOL, DINO 처럼 self-disitillation 구조를 사용함
> - Teacher network와 student network를 사용하며, 두 networks의 구조는 동일함
> - Teacher network 파라미터는 student network 파라미터의 exponential moving average로 업데이트됨
- 각 입력 이미지로부터 augmentations가 적용된 두 개의 views가 만들어지며, 각각에 대해 마스킹을 적용한 이미지도 생성함
- MIM objective는 두 views의 original views와 masked views가 각각 주어졌을 때, masked된 view에 대한 student network의 output과 대응되는 원본 view에 대한 teacher network의 out의 차이를 최소화하는 것으로 정의됨
- 또한, 서로 다른 views의 original view와 masked view에 대해서도 동일하게 loss를 계산하되 CLS token을 사용하여 global하게 적용시킴 (DINO의 loss function)
- Teacher network의 attention map을 기반으로 AttMask 전략을 사용함
> - CLS token을 기준으로 highly-attended patches 일부를 마스킹함

## 실험결과
- Masking strategies의 효과를 검증하기 위해 iBOT에 AttMask를 적용한 결과 random masking을 사용했을 때보다 성능이 더 좋았음
- DINO에 AttMask를 적용하되 MIM loss를 사용하지 않았을 때 성능은 default DINO보다 좋았음
> - AttMask는 MIM 목적을 차치하고서라도 좋은 method임
- iBOT에 AttMask를 추가했을 때 여러 benchmark 평가에서 기존 iBOT 보다 좋은 성능을 기록함

## 의견
- /