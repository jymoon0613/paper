# MixMAE: Mixed and Masked Autoencoder for Efficient Pretraining of Hierarchical Vision Transformers (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2205.13137)

<p align="center">
    <img width="800" alt='fig1' src="../img/liu2023mixmae.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Masked image modeling(MIM)은 여러 vision tasks에서 활발히 사용되는 self-supervised visual representation learning method임
- MIM은 vanilla ViT에서 괄목할만한 성능 향상을 기록하였지만 최근의 hierarchical ViT에서도 성능 향상을 이끌 수 있는지에 대해서는 깊게 탐구되지 않았음
- 현재의 MIM은 입력 토큰의 밀부를 특수한 MASK 토큰으로 대체하고 모델이 원본 이미지 패치를 reconstruction하도록 설계됨
- 하지만 MASK 토큰의 사용은 두 가지 문제를 야기할 수 있음
> - Pre-training에서 사용되는 MASK 토큰은 fine-tuning 시에는 사용되지 않으며 이는 pre-train과 fine-tune 간의 불일치를 야기함
> - 모델은 정보가 적은 MASK 토큰을 처리하기 위해 너무나 많은 연산을 낭비하게 되고 이는 훈련 과정을 비효율적으로 만듦
> - 이러한 문제들은 masking ratio가 커질수록 심각해짐
- MAE는 위와 같은 문제를 겪지 않는데 이는 encoder에서는 MAE가 MASK 토큰을 처리하지 않고 오직 가벼운 decoder에서만 처리하도록 설계되었기 때문임
- 하지만 MAE는 vanilla ViT encoder를 사용할 때만 적용될 수 있다는 한계가 있음
> - Hierarchical ViT는 일반 ViT와 달리 임의의 길이의 1D token sequences를 처리할 수 없음
- 따라서, 본 논문에서는 generalized된, 범용적인 pre-training method인 mixed and masked autoencoder(MixMAE)를 제안함
> - MixMAE는 hiearchical ViT의 pre-training에 사용될 수 있음

## 접근법
- 이전의 MIM은 pre-training 시 많은 수의 epochs를 필요로 하는데, 이는 정보가 빈약한 MASK 토큰을 처리하는 데 너무나 많은 연산을 허비하기 때문임
- 또한, MASK 토큰은 오직 pre-training 시에만 사용되며 이는 pre-training과 fine-tuning 간의 불일치를 야기함
- 이러한 문제를 해결하기 위해, MixMAE는 서로 다른 unlabelled images를 섞은 mixed images를 pre-training의 입력으로 사용함
> - 모델은 mixed input으로부터 original images를 reconstruction하도록 훈련됨
> - 즉, 기존의 MASK 토큰을 다른 이미지의 패치로 대체한 것과 같음
> - MASK 토큰을 사용하지 않으므로 연산량의 낭비가 적어지고 fine-tuning과의 불일치를 제거할 수 있음 (더 나은 downstream 성능으로 이어짐)
- 여러 hiearchical ViT를 encoder로 사용할 수 있지만 본 논문에서는 Swin Transformer를 encoder로 사용
- Mixed input을 encoder가 인코딩한 후, 인코딩된 mixed input은 섞인 이미지 각각으로 다시 unmix됨
- Unmix된 이미지에서 다른 이미지 토큰으로 대체되었던 부분은 MASK token으로 대체해줌
- MAE와 마찬가지로 lightweight decoder는 unmix되고 MASK token으로 대체된 input을 각자의 원본 이미지로 reconstruction함
- Optimization을 가속화하기 위해 다음과 같은 추가적인 고려가 적용됨
> - Positional embedding과 함께 섞인 이미지 패치들이 각각 서로 다른 이미지에서 추출되었다는 것을 명시적으로 구분하기 위해 mix embedding을 추가해줌
> - Self-attention 시, 서로 같은 이미지로부터 추출된 토큰에 대해서만 attention을 수행함

## 실험결과
- MixMAE로 pre-training한 뒤, classification, detection, segmentation tasks로 fine-tuning한 성능을 평가함
- ImageNet-1K를 사용하여 pre-training함
- 다른 MIM methods와 비교했을 때 MixMAE는 더 적은 epochs로 더 좋은 성능을 달성함
- SwinT 이외의 다른 hierarchical ViT를 사용해도 성능이 좋음

## 의견
- /