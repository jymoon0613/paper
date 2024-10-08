# SimMIM: A Simple Framework for Masked Image Modeling (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Xie_SimMIM_A_Simple_Framework_for_Masked_Image_Modeling_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/xie2022simmim.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MIM을 위한 아주 간단한 프레임워크를 제안함 (SimMIM)
> - 이전의 방법들과 달리 매우 간단하지만 representation 학습 측면에서 유사하거나 더 좋은 성능을 보임

## 접근법
- SimMIM framework는 4가지 주요 구성요소로 이루어져 있음
> - Masking strategy, encoder architecture, prediction head, prediction target
- 다양한 masking strategies를 사용하여 실험을 진행함
> - 이미지 패치 단위로 랜덤하게 마스킹하는 patch-aligned random masking strategy를 사용함 (simple random masking)
> - Context encoder에서 제안된 square masking, BEiT의 block-wise masking도 사용해봄
- Encoder architecture로는 ViT와 Swin-T를 사용함
- 본 논문에서는 prediction head로 이전의 무거운 decoder 구조가 아닌 매우 가벼운 linear layer를 사용함
- Prediction target으로는 MAE와 마찬가지로 raw pixel-level values를 사용함

## 실험결과
- Simple random masking을 사용했을 때 성능이 가장 좋았음
> - 특히, 큰 masked patch size(i.e., 32)를 사용했을 때 전반적으로 더 좋았음
> - 이는 masked patch size가 커질수록 이미지의 locality가 사라지므로 모델이 long-range connection에 더 집중하도록 할 수 있음을 의미함
> - 또한, masking ratio가 커질수록 전반적으로 성능이 향상되었음
- 간단한 prediction head를 사용해도 성능이 준수했음
> - 무거운 prediction head를 사용했을 때 pre-training loss가 더 낮았지만 transferring 성능은 오히려 안좋았음
> - 이는 pre-trainin 시의 inpainting 정확도가 꼭 좋은 downstream 성능을 보장하는 것은 아님을 의미함
> - 따라서, prediction head를 무겁게 설계하여 computation을 낭비할 필요가 없음  
- Raw pixel-level values를 prediction targets으로 설정해도 충분히 좋은 성능을 낼 수 있음
- 전체 이미지 패치를 고려하는 것보다 masked patches에 대해서만 reconstruction loss를 고려하는 것이 성능이 더 좋았음
- DINO, BEiT, MoCov3와 비교했을 때도 성능이 가장 좋았음
- SwinT를 backbone으로 사용했을 때도 평가했으며 같은 모델 사이즈의 supervised models보다 성능이 좋았음

## 의견
- /