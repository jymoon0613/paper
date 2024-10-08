# InternImage: Exploring Large-Scale Vision Foundation Models With Deformable Convolutions (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Wang_InternImage_Exploring_Large-Scale_Vision_Foundation_Models_With_Deformable_Convolutions_CVPR_2023_paper.html)

<p align="center">
    <img width="300" alt='fig1' src="../img/wang2023internimage.png?raw=true"></br>
    <em><font size=2>Overall Architecture of InternImage.</font></em>
</p>

## 연구목적
- 최근 ViT를 scale-up하려는 많은 시도가 있었으며, 그 결과 ViT 기반 모델은 여러 computer vision tasks에서 CNN 기반 모델의 성능을 능가하고 있음
- 본 논문에서는 CNN 기반 모델이 ViT와 유사한 operator/architecture-level 디자인, scaling-up parameters, 방대한 훈련 데이터를 취할 수 있으면 오히려 ViT보다 더 좋은 성능을 낼 수 있다고 주장함
- 이를 증명하기 위해, 본 논문에서는 CNN과 ViT를 operator, architecture 등의 관점에서 비교하며, CNN을 parameter/data 측면에서 scaling-up할 수 있는 효과적인 방법을 탐구함
> - 이 과정에서 deformable convolution(DCN)을 적극적으로 활용함
- 그 결과 새로운 CNN 백본 네트워크인 InternImage를 제안함

## 접근법
- CNN의 convolution과 달리, ViT의 multi-head self-attention(MHSA)은 long-range dependencies와 adaptive spatial aggregation 측면에서 강점이 있음
- 따라서, convolution과 MHSA의 격차를 줄이기 위해서는 long-range dependencies와 adaptive spatial aggregation의 특성을 convolution에 주입할 필요가 있음
- 이러한 측면에서 DCN(DCNv2)은 offset sampling과 learnable modulation scalar로 구성되며 MHSA와 유사한 특성을 지님
- 본 논문에서는 기존의 DCN을 확장하여 large-scale visual model에 더 적합한 형태로 변환함
> - 기존 convolution weights를 depth-wise part, point-wise part로 구분하여 연산 효율성을 개선함 (separable convolution의 개념을 차용)
> - Convolution이 수행되는 영역을 나누어 multi-group convolution을 수행하도록 하여 spatial aggregation patterns를 다양화함
> - Normalizing 방식을 변경하여 training 안정성을 높임
- 그 결과 새로운 DCN(DCNv3)는 long-range dependencies와 adaptive spatial aggregation 측면에서 MHSA와 비슷한 특성을 가지면서, convolution의 효과적인 inductive bias를 그대로 지니고 있으며, 연산량과 메모리 측면에서도 효율적임
- DCNv3를 기반으로 InternImage 아키텍처를 설계함
> - DCNv3와 ViT의 발전된 요소들(i.e., GELU, LN)을 반영하여 basic block 설계
> - 효과적인 scale-up을 위해 stem/downsampling layers, stacking/scaling rules를 설계

## 실험결과
- Classification, detection, segmentation tasks에서 기존의 CNN-/Transformer-based 모델보다 좋은 성능을 기록하여 뛰어난 large-scale visual foundation model임을 증명

## 의견
- /