# Masked-Attention Mask Transformer for Universal Image Segmentation (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Cheng_Masked-Attention_Mask_Transformer_for_Universal_Image_Segmentation_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/cheng2022masked.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Semantic segmentation에서는 per-pixel classification 아키텍처가 주로 사용된 반면, instance semgentation에서는 mask classification 아키텍처가 주로 사용됨
- 최근에는 모든 segmentation tasks에 적용될 수 있는 universal한 아키텍처가 제안되고 있음
> - 주로 DETR, MaskFormer와 같이 end-to-end set prediction objective에 기반하고 있음
- Universal 아키텍처는 매우 flexible하고 성능도 준수하지만 여전히 specialized 아키텍처에 비해 성능이 약간 뒤처진다는 한계가 있음
> - 또한 training이 어렵다는 문제도 있음
- 따라서, 본 논문에서는 모든 segmentation tasks에서의 specialized 아키텍처의 성능을 뛰어넘으면서 training이 쉬운 universal segmentation 아키텍처인 masked-attention mask Transformer(Mask2Former)를 제안함
> - Mask2Former는 Transformer decoder에서 masked attention를 사용하여 빠른 모델 수렴과 향상된 성능을 기록함
> - Multi-scale high-resolution features를 사용하여 작은 objects/regions도 잘 segment할 수 있도록 함
> - Optimization 향상을 위한 추가적인 기법들을 제안함
> - Training 시 mask loss를 일부 랜덤하게 샘플링된 points에서만 계산하여 training memory를 효과적으로 줄임(성능 영향 없이) 

## 접근법
- Mask2Former는 backbone feature extractor, pixel decoder, Transformer decoder로 구성됨
> - MaskFormer 구조와 동일
- 단, Mask2Former의 Transformer decoder는 masked attention이 추가됨
> - Cross-attention 수행 시 이전의 Transformer decoder의 mask prediction을 바탕으로 마스킹을 수행
> - 전체 feature map에 attend하는 것이 아니라 각 query에 대해 예측된 마스크의 foreground region으로 cross-attention을 제한시킴으로써 localized features를 추출
- 작은 objects/regions에 대한 segmentation 성능을 높이기 위해 multi-scale 전략을 사용함
> - 연산량을 고려하여, low/high resolution features로 구성된 feature pyramid를 만들고 Transformer decoder layer 하나당 매번 한 resolution의 feature map을 전달함 
> - Pixel decoder는 3개의 다른 resolution을 가지는 multi-scale feature maps를 출력
> - 하나의 Transformer decoder는 3개의 Transformer decoder layer로 구성되며 각 decoder layer는 3개의 multi-scale feature maps를 순차적으로 입력받음
- Optimization 향상을 위한 추가적인 기법들을 제안함
> - 먼저, self-attention과 cross-attention(masked attention)의 순서를 변경함
> - Query features를 decoder에 입력하기 전에 첫번째 mask를 예측하기 위해 사용하고 supervised하게 학습시킴
> - Decoder에서 dropout을 제거함
- Training의 효율성을 높이기 위해 mask loss는 K개의 random하게 선택된 points에서만 계산됨

## 실험결과
- Panoptic segmentation의 annotations으로만 훈련해도 instance/semantic segmentation을 위해 사용될 수 있음
> - Mask2Former의 universality를 증명
- Panoptic/instance/semantic segmentation tasks에서 모두 specialized 아키텍처보다 좋은 성능 기록

## 의견
- /