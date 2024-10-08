# SdAE: Self-distillated Masked Autoencoder (ECCV, 2022)

[논문링크](https://arxiv.org/abs/2208.00449)

<p align="center">
    <img width="600" alt='fig1' src="../img/chen2022sdae.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT의 도입과 함께 generative-based self-supervised learning 기법 중 하나인 MIM이 각광받고 있음
- 대표적인 방법 중 BeiT, PeCo는 masked tokens의 latent representation을 얻기 위해 pre-trained feature descriptor(i.e., dVAE)가 필요함
> - 'Pre-pretraining'가 요구됨
- MAE는 encoder-decoder 구조를 사용하여 latent representation으로부터 raw pixels를 reconstruct하며, MaskFeat은 image features를 tokenize하기 위해 hand-crafted feature descriptor인 HOG를 사용함
> - Pre-pretraining 과정이 필요하지는 않지만 pixel-level의 low-level representations을 reconstruct하는 것은 high semantic level tasks에 불필요할 수 있음
> - 또한, pre-training stage와 downstream tasks에서의 optimization gap을 유발할 수 있음
> - 즉, 좋은 reconstruction quality가 꼭 모델의 high descriptive capability를 의미하는 것은 아님
- 본 논문에서는 이러한 문제들을 고려하여 self-distillated masked autoencoder structure를 제안함(SdAE)
> - SdAE는 self-distillated teacher-student 아키텍처를 사용하여 reconstruction target이 되는 latent representation을 생성함

## 접근법
- Self-distillated teacher-student 아키텍처를 사용함
> - Student network와 teacher network는 동일한 구조의 encoder를 사용하며, student network만 shallow decoder를 포함하고 있음
> - Teacher network의 파라미터는 student network 파라미터의 exponential moving average로 업데이트됨
- 마스킹된 image에 대한 student network의 reconstructed latent representation과 원본 이미지에 대한 teacher network의 latent representation 간의 cosine similarity를 최대화하도록 훈련됨
- 하지만, teacher network에 원본 이미지를 입력하는 것은 sub-optimal할 수 있음
> - Teacher network도 마스킹된 이미지를 사용할 때 성능이 더 좋았음 (teacher crop)
- 따라서, 본 논문에서는 일부 teacher crops를 그룹화하여 충분한 masked tokens를 사용하기 위해 multi-fold masking strategy를 제안함
> - 하나의 이미지는 서로 다른 랜덤 마스크로 마스킹된 t개의 fold로 나뉘어짐 (마스크 위치는 overlap X)
> - 각 fold는 독립적으로 공유되는 teacher network로 입력됨
> - 각각의 계산된 latent representations는 student network의 output과의 loss 계산에 사용됨

## 실험결과
- ImageNet classification task에서 MAE, BEiT 등 기존의 SSL 방법들보다 좋은 성능 기록
> - Supervised training from scratch보다도 성능이 좋았음
- Semantic segmentation, object detection에서도 좋은 성능 기록

## 의견
- /