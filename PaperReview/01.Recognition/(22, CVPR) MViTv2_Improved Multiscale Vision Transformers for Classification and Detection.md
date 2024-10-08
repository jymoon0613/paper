# MViTv2: Improved Multiscale Vision Transformers for Classification and Detection (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Li_MViTv2_Improved_Multiscale_Vision_Transformers_for_Classification_and_Detection_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/li2022mvitv2.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transformer내의 Self-Attention block은 input scale에 따라 연산량과 메모리가 quadratic하게 증가하는 문제가 있음
- 이러한 부담을 해결하기 위해 Swin Transformer는 window 기반의 local attention 방법을 제안하였고, MViT는 pooling attention 방법을 제안함
- MViT는 multiscale hierarchy의 개념을 도입하여 video recognition tasks에서 SOTA를 달성했음
- 본 논문에서는 기존의 MViT에 간단한 technical improvements를 적용하여  모두에서의 성능 향상을 도모함 (MViTv2)

## 접근법
### (1) Improved Pooling Attention
- MViT는 기존 ViT의 absolute positional encoding을 사용함
- 하지만, MViT는 pooling attention을 통해 각 stage별로 resolution이 감소하게 되고, 이때 patch 간의 상대적인 위치가 변하지 않더라도 absolute position encoding 값이 바뀌게 됨
- 따라서, pooled self-attention을 하는 과정에서 input patch 사이의 pair-wise relationship을 고려하여 상대적인 위치를 연산하는 relative positional encoding을 적용함
- Self-attention 시 두 패치의 상대적인 위치를 query에 반영
- Self-attention 이후 pooled query를 output에 더해줌 (residual pooling connection)

### (2) MViT for Object Detection
- MViT는 각 Stage별로 multiscale feature maps을 생성하므로 object detection을 위한 FPN을 적용함
- Swin Transformer의 local attention과 pooling attention을 결합한 hybrid window attention (Hwin) 사용

## 실험결과
- Image classification, object detection, video classification에서 SOTA 성능 달성

## 의견
- /