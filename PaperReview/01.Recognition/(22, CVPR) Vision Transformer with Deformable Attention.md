# Vision Transformer with Deformable Attention (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Xia_Vision_Transformer_With_Deformable_Attention_CVPR_2022_paper.html?ref=https://githubhelp.com)

<p align="center">
    <img width="400" alt='fig1' src="../img/xia2022vision.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Plain ViT의 self-attention은 computational cost는 매우 크며 이러한 문제를 해결하기 위한 방법들이 최근에 많이 제시되었음
> - Swin Transformer(SwinT): local attention -> hand-crafted attention patterns은 data-agnostic하며 suboptimal할 수 있음
> - Pyramid Vision Transformer(PvT): key, value downsampling -> 중요한 key/value 정보가 손실될 수 있음
- 가능한 해결책 중 하나는 주어진 query에 대해 최적의 key/value set을 찾아 attention을 수행하는 것
> - Deformable Convolution Networks(DCN)에서 convolution filter가 data-dependent한 방식으로 더 informative한 영역에 선택적으로 집중할 수 있도록 deformable한 receptive fields를 학습하는 것과 같음
> - 이전에도 deformable mechanism을 Transformer에 적용하려고 했던 시도가 있었지만 deformable offsets 계산이 패치 개수에 quadratic하기 때문에 메모리/계산 복잡도가 매우 커졌고 일부 layer/head 부분에만 적용되었음
- 따라서, 본 논문에서는 간단하면서 효율적인 deformable self-attention module을 제안하고, 이것을 basic building block으로 사용하는 Deformable Attention Transformer(DAT)를 설계함
- DCN은 feature map의 모든 pixel 위치에서 서로 다른 offsets을 학습했지만, DAT는 모든 query가 공유하는 몇 가지 offsets group을 학습함
> - 선행 연구로부터 global-attention이 일반적으로 서로 다른 query에 대해서도 거의 동일한 attention pattern을 보인다는 점에 착안함

## 접근법
- 우선, 입력 feature map에 대해 uniform한 grid points를 생성함
- 동시에 입력 feature map으로부터 query를 계산하고, query는 subnetwork를 거쳐 offset을 계산함
- 계산된 offsets은 reference grid points에 더해지고, bilinear interpolation을 통해 offsets이 적용된 위치에서의 feature를 샘플하여 key, value를 계산함
- 이후 self-attention 진행
- Multi-head attention과 비슷하게 feature channel을 G개의 group으로 구분하고, 같은 group에서는 같은 subnetwork를 공유하도록 함
- Deformable self-attention으로 구성된 DAT는 hierarchical한 Transformer의 구조를 사용함
- Deformable self-attention은 마지막 두 stage에서만 사용됨 (초기 stage은 Swin Transformer의 shift-window attention을 사용)

## 실험결과
- Classification, detection, segmentation에서 모두 좋은 성능을 보임

## 의견
- Figure.5 참고해서 시각화해볼 수 있을듯