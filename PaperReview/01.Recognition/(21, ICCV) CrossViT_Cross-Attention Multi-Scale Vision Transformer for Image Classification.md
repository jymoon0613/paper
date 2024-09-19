# CrossViT: Cross-Attention Multi-Scale Vision Transformer for Image Classification (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Chen_CrossViT_Cross-Attention_Multi-Scale_Vision_Transformer_for_Image_Classification_ICCV_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/chen2021crossvit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 여러 vision task에서 multi-scale feature representation의 효과가 입증되었으며, 이러한 효과는 ViT에서도 적용될 수 있음
- 따라서, 본 논문에서는 multi-scale feature representation을 학습하는 visual Transformer를 제안함 
> - Dual-branch Transformer 구조를 사용
> - 두 개의 분리된 branch로 small image patch와 large image patch가 각각 입력되며 이들은 서로를 complement하기 위해 여러번 fuse되어 이미지 분류를 위한 더욱 강한 표현을 생성함 (feature fusion)
> - 이러한 fusion은 cross-attention module을 통해 이루어지며, 각 Transformer branch는 attention을 통해 다른 분기와 정보를 교환하기 위해 non-patch token을 생성하여 사용함 (CrossViT)

## 접근법
- 일반적으로 ViT에서 fine-grained한(작은) patch size를 사용할수록 더 높은 성능을 얻을 수 있지만 FLOPs와 메모리 소모량도 그만큼 커짐
- 따라서, CrossViT는 서로 다른 브랜치에서 서로 다른 크기의 image patch를 처리하되, 두 브랜치의 복잡도 차이를 고려하여 block의 개수를 조절함
- 구체적으로, CrossViT는 K개의 multi-scale transformer encoders로 구성되었으며, 각각의 encoder는 두 개의 branch로 구성됨
> - L-branch: primary branch로서 coarse-grained patch size를 처리하며 transformer encoder의 수가 많고 embedding dimension이 큼
> - S-branch: complementary branch로서 fine-grained patch size를 처리하며 transformer encoder의 수가 적고 embedding dimension이 작음
> - 두 브랜치는 L번 fuse되며 두 브랜치의 CLS token이 최종적으로 분류를 위해 사용됨
> - 두 브랜치 각각에 positional embedding 적용
- 이때 두 branch를 fusion하는 방법은 다양함
> - (1) 두 브랜치의 차원을 동일하게 projection하여 전체를 concat한 sequence에 self-attention 적용
> - (2) 두 브랜치의 CLS token의 차원만을 동일하게 projection하고, 두 CLS token을 더한 CLS token을 각 branch의 새로운 CLS token으로 사용하여 self-attention 적용
> - (3) 위와 마찬가지로 CLS token을 서로 더하여 fusion하되, 두 브랜치의 image token도 spatial position(input sequence에서의 순서)에 맞게 각각 더해준 뒤 self-attention 적용
> - (4) 두 브랜치의 CLS token의 차원을 projection을 통해 counterpart의 차원으로 만들어주고, 서로의 CLS token을 교환하여 self-attention 적용 (proposed cross-attention)
- Cross-attention 방법은 두 브랜치의 CLS token을 정보 교환을 위한 agent로 사용함
- CLS token은 각자의 브랜치에서 전반적인 image patch의 정보를 담고 있으며 cross-attention을 통해 서로 다른 scale의 image patch 정보도 담을 수 있음
- Cross-attention을 통한 fusion 이후 다음 transformer block에서 CLS token은 각자의 branch로 복귀하며, 이는 CLS token이 conterpart 브랜치에서 학습한 다른 scale의 정보를 현재의 image patch들에 전달할 수 있다는 것을 의미함
- 실제 구현에서 cross-attention의 query는 projection된 CLS token의 것만 사용함 (계산 및 메모리 비용이 낮음)

## 실험결과
- ImageNet1K에서 비슷한 capacity의 DeiT보다 좋은 classification 성능 기록
- 다른 (당시)최신의 ViT variants 및 CNN 모델과 비교했을 때도 좋거나 경쟁력있는 성능을 기록 (accuracy, FLOPs, throughput 측면에서)

## 의견
- /