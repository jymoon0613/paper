# Mixed Autoencoder for Self-supervised Visual Representation Learning (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2303.17152)

<p align="center">
    <img width="600" alt='fig1' src="../img/chen2023mixed.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT의 등장 이후 NLP의 masked language modeling(MLM)에서 영감을 받은 masked image modeling(MIM)이 주요한 self-supervised methods로 떠올랐음
> - MIM은 입력 이미지의 패치 일부를 마스킹한 뒤 visible한 패치을 기반으로 마스킹된 패치들을 reconstruction하는 방식
> - 이러한 과정에서 encoder는 여러 downstream tasks에서 유용한 매우 semantic한 representation을 학습함
- MIM에 대한 기존의 연구는 주로 reconstruction targets(i.e. visual tokenizers, pixel-level), masking strategy(i.e. random, attention-guide)에 관해서 발전이 있었음
- 하지만, MIM을 위한 input augmentation에 대해서는 거의 탐구되지 않았음
> - Contrastive learning의 대표적인 augmentation method인 color jitter를 masked autoencoder(MAE)에 추가했을 때 transfer results가 오히려 감소했음
> - 이는 MIM이 data augmentation에 대해 기존과는 다른 양상을 보이고 있다는 것을 의미하며 MIM을 위한 효과적인 data augmentation에 대한 연구가 필요함을 암시함
- 본 논문에서는 supervised training 및 contrastive learning에서 주로 쓰이는 augmentation 방법인 image mixing을 MAE에 적용해봤음
> - Image mixing을 MAE에 수정 없이 바로 적용했을 때 발생하는 문제를 정의하고 이를 극복하기 위한 auxiliary pretext task인 homologous recognition을 제안함
> - 이를 바탕으로 최종적으로 Mixed Autoencoder network(MixedAE)를 설계함 (MixedAE)

## 접근법
- 우선 image mixing을 MAE에 바로 적용해봄
> - 한 배치의 모든 입력 이미지에 대해 patch embedding을 수행함
> - 배치 내의 이미지를 그룹화함
> - 같은 그룹 내의 이미지를 일정하게 섞어서 각 그룹마다 mixed image를 생성함 (random mixing mask 이용)
> - Mixed image는 encoder를 거쳐 인코딩됨
> - 이후 mixed image의 representation은 unmixed됨
> - 이때 mixed image의 representation은 그것을 구성하는 개별 이미지 representation으로 나뉘어지고, 한 representation에서 다른 이미지의 representation이 섞였던 부분은 MASK token으로 대체됨
> - Decoder는 unmixed representation을 바탕으로 unmixed image 각각을 reconstruction함
- 하지만 위와 같은 단순 적용은 오히려 성능을 감소시켰음
- 이는 위와 같은 적용이 reconstruction 과정을 쉽게 하기 때문임
> - MAE의 패치 마스킹은 일부를 제외한 이미지 패치를 MASK token으로 대체해버리기 때문에 모델 입력과 reconstruction target 간의 상호 정보를 줄임
> - 반면 위의 단순 mixing 적용은 오히려 mix된 이미지로부터 추가 정보를 얻을 수 있으므로 모델 입력과 reconstruction target 간의 상호 정보를 늘려버릴 수 있음
- 또한 ViT의 global self-attention도 모델 입력과 reconstruction target 간의 상호 정보를 늘려버림으로써 reconstruction 과정을 쉽게 만듦
> - Global self-attention 과정에서 한 이미지의 패치는 다른 이미지의 패치와 interaction을 계산하게 되고, 이 과정에서 다른 이미지로부터 의도치 않은 정보를 전달받을 수 있음 
- 따라서, 이를 해결하기 위해 각 쿼리가 같은 이미지로부터 추출된 패치에 대해서만 attend하도록 하는 auxiliary pretext task인 homologous recognition을 제안함
- 각 query에 대해 key와의 attention mass를 계산한 뒤, 상위 Top-K개의 attention mass를 지니는 key에 대해서만 attention score을 수행함 (homologous attention)
- 이때 Top-K 샘플링 시 실제로 같은 이미지로부터 나온 patch들이 attend될 수 있도록 homologous contrastive loss를 사용함
> - 모든 block을 거친 representation에 대해 homologous patches의 representation은 유사해지도록, heterologous patches에 대해서는 dissimilar해지도록 cosine similarity 기반의 loss를 정의
- 또한, homologous recognition을 강화하기 위해 segment embedding을 추가로 사용
> - Mixed된 이미지에서 서로 같은 이미지로부터 샘플링된 패치에 일정한 값을 더해주어서 각 패치가 서로 같은/다른 이미지로부터 샘플링되었다는 것을 표시
- 최종 loss는 reconstruction loss + homologous contrastive loss로 정의
- MixedAE는 object-aware self-supervised pre-training임
> - ImageNet은 하나의 이미지에 하나의 object가 존재
> - ImageNet 이미지 여러 개를 mix하면 그만큼의 object가 mixed된 이미지에 존재하는 것 ('pseudo' multi-instance image)
> - 이때 MixedAE는 homologous recognition으로 인해 서로 같은 이미지로부터 샘플링된 패치의 존재를 학습해야 하며 이는 mixed된 이미지에 존재하는 모든 object에 대한 정보를 학습해야 한다는 것임

## 실험결과
- ImageNet-1K classification을 포함한 여러 tasks에서 좋은 성능을 기록
> - 대부분의 tasks에서 MAE를 포함한 여러 방법들의 성능을 넘음

## 의견
- /