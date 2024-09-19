# Tokens-to-Token ViT: Training Vision Transformers from Scratch on ImageNet (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Yuan_Tokens-to-Token_ViT_Training_Vision_Transformers_From_Scratch_on_ImageNet_ICCV_2021_paper.html?ref=https://githubhelp.com)

<p align="center">
    <img width="500" alt='fig1' src="../img/yuan2021tokens.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 large-scale dataset이 아닌 midsize dataset으로 훈련했을 때 CNN보다 성능이 낮았음
- 본 저자들은 이러한 성능 차이가 ViT의 두 가지 주요한 한계점에 의해 발생한다고 주장함
> - (i) 단순히 입력 이미지를 일정한 크기로 split하는 tokenization 방식으로 인해 ViT는 edges, lines와 같은 image의 local structure를 모델링할 수 없으며, 이는 CNN과 유사한 성능을 내기 위해 훨씬 많은 훈련 데이터를 요구하게 됨
> - 실제 ResNet과 ViT의 feature를 시각화해본 결과 ResNet은 bottom layer부터 middle layer에 이르기까지 이미지의 local structure를 잘 모델링한 반면, ViT는 global relations는 모든 block에 걸쳐 잘 모델링했지만 local sturcture와 관련된 features는 매우 poor했음
> - (ii) ViT의 self-attention block은 CNN만큼 vision tasks에 최적화되지 않았으며, 이로 인해 self-attention은 많은 redundancy를 내포하여 feature richness를 제한하고 model training을 어렵게 함
> - 위와 같은 시각화 결과에서 ViT의 많은 channels는 0에 가까운 값을 가지고 있었으며 이는 ViT의 backbone, 즉, self-attention block이 데이터가 충분하지 않은 상황에서 ResNet(CNN)만큼 효율적이지 못하고 제한적인 feature richness를 제공하고 있다는 것을 의미함
- 따라서, 본 논문은 위의 문제를 해결하기 위한 새로운 visual Transformer 구조를 제안함
> - Naive한 tokenization 대신 점진적으로 이웃하는 tokens를 하나의 token으로 aggregate하는 tokens-to-token (T2T) module을 사용함
> - 이러한 방식은 반복적으로 tokens의 길이를 점차 줄이면서 tokens 주변의 local structure를 모델링할 수 있도록 함
> - 또한 feature richness를 향상시킬 수 있는 효율적인 ViT bacbone을 설계하기 위해 CNN의 아키텍처 디자인 요소를 일부 도입하여 deep-narrow 아키텍처를 고안함
> - Deep-narrow 아키텍처는 channels 수를 줄이는 대신 layers 수를 증가시키며, 이를 통해 비슷한 모델 크기와 연산량으로 더 좋은 성능을 낼 수 있었음
- T2T module과 deep-narrow backbone 아키텍처를 사용하여 vanilla ViT보다 가벼우면서 성능이 뛰어난 tokens-to-token vision Transformer (T2T-ViT)를 설계함

## 접근법
- T2T module는 re-structurization과 soft split의 두 단계로 진행됨
- Re-structurization은 Transformer encoder block을 통과한 tokens를 reshape하여 2D 형식으로 shape을 맞춰주는 과정을 의미함
> - 즉, output tokens $T^{\prime}\in{R^{l\times{c}}}$는 $I^{\prime}\in{R^{h\times{w}\times{c}}}$로 reshape되며, 이때 $l=h\times{w}$임
- Re-structurized image $I$에 대해 soft split을 적용하여 local structure information을 모델링하고 tokens의 길이를 줄임
> - $I$를 overlapping patches로 split함
> - Surrounding tokens는 서로 강한 상관관계를 지니기 때문에 soft split으로 인해 생성되는 surrounding patches도 강한 상관관계를 가지고 있음
> - 각 split에 속하는 tokens를 하나의 token으로 concat하며, local information은 surrounding pixels와 patches로부터 aggregate될 수 있음
- 또한, CNN 디자인에서 착안하여 deep-narrow라는 T2T-ViT backbone design을 고안함
> - ViT block에서, channel redundancy를 줄이기 위해 channel dimensions를 줄이고 feature richness를 향상시키기 위해 layer depth를 증가시킴
- T2T-ViT는 T2T module과 T2T-ViT backbone으로 구성됨
> - T2T module에서는 가벼운 Transformer encoder block과 T2T re-structurization, soft split을 반복적으로 적용함
> - Deep-narrow 아키텍처에 기반한 T2T-ViT backbone은 T2T module의 output tokens을 입력으로 받아 positional encoding을 적용하고 반복적으로 처리하여 최종 prediction을 수행함

## 실험결과
- Midsize dataset인 ImageNet에서 모델을 평가함
- T2T-ViT는 기존 ViT 보다 훨씬 적은 연산량으로 더 좋은 ImageNet 분류 성능을 기록함
- 또한, 비슷한 크기의 ResNet, MobileNet과 성능이 거의 비슷하거나 더 좋았음

## 의견
- / 