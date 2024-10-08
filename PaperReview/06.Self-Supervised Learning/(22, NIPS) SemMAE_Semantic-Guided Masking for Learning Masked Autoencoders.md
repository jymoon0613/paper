# SemMAE: Semantic-Guided Masking for Learning Masked Autoencoders (NIPS, 2022)

[논문링크](https://arxiv.org/abs/2206.10207)

<p align="center">
    <img width="800" alt='fig1' src="../img/li2022semmae.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MIM은 좋은 성능을 보여주고 있지만 MLM과 비교했을 때 아직 많은 차이가 존재함
> - NLP의 sentence는 그 semantics가 words로 decompose될 수 있지만, CV의 이미지는 그 semantics를 decompose하기 어려움
- 본 논문에서는 words와 유사한 image의 decomposed features를 찾기 위해 part-based image representation에 집중함
> - 이미지는 여러 objects로 구성되어 있고, 각 object는 또 서로 다른 parts로 구성되어 있음
> - 즉, part-based image representation은 이미지의 semantic properties를 decompose하는 기준이 되기 충분함
- 이러한 part-based image representation을 적절히 활용한다면 MAE 학습을 가이드 할 수 있는 더 controllable한 hints를 만들 수 있고, high-level visual representations 학습을 더욱 boost할 수 있음
- 따라서, 본 논문에서는 semantic parts를 기반으로 MAE 학습을 boost하는 Semantic-guided masked autoencoders를 제안함(SemMAE)

## 접근법
- SemMAE는 두 가지 key components로 구성됨: Semantic Part Laerning, Semantic-Guided Masking
> - Semantic part learning에서는 입력 이미지에 존재하는 object의 part segmentation maps를 획득하는 방법을 학습함
> - Semantic-guided masking은 part segmentation maps를 바탕으로 MAE의 mask generation을 가이드함
> - 각 part를 구성하는 patches 일부를 마스킹하거나, 각 part 전체를 한번에 마스킹하는 등의 전략이 사용될 수 있음
- Semantic part learning을 수행하기 위해, 먼저 iBOT으로 사전 훈련된 ViT 모델로 입력 이미지의 features를 추출함
- Output features를 CLS token과 patch tokens로 분리하고, CLS token은 N개의 part tokens로 임베딩됨
> - Part tokens는 CLS token의 channels를 semantics 기준으로 grouping하는 역할을 함
- Patch tokens와 part tokens의 correlation function을 기반으로 각 position에서 semantic part가 나타날 확률을 모델링하는 attention maps를 구함
- Attention maps를 optimize하기 위해 reconstruction task를 사용함
> - Attention maps(spatial information)와 CLS token(texture information)을 사용하여 원본 이미지를 reconstruction함
> - StyleGAN-based decoder를 사용함
- 또한, attention maps가 spatially diverse해질 수 있도록(각 attention map이 서로 다른 region에 attend하도록) attention maps에 diversity constraint를 추가함
- Semantic part learning 이후, part attention maps에 기반하여 semantic-guided MAE training을 실시함
- Part attention maps에 argmax 연산을 수행하여 part segmentation map을 얻음
> - 각 이미지 패치는 특정한 semantic part로 분류됨
- 학습 초기에는 각 semantic parts의 일부가 마스킹됨
> - 모델은 각 semantic parts의 남아 있는 visible patches를 참조하여 masked patches를 예측 (intra-part patterns)
- 이후 점차적으로 특정 parts의 모든 patches를 마스킹함
> - 모델은 서로 다른 parts를 바탕으로 masked parts를 예측 (inter-part relations)

## 실험결과
- SemMAE는 linear probing에서 MAE보다 좋았음
- 시각화를 해본 결과 iBOT보다 semantic part learning 퀄리티가 좋았음

## 의견
- /