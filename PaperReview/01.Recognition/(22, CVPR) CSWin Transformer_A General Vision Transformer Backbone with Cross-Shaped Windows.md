# CSWin Transformer: A General Vision Transformer Backbone with Cross-Shaped Windows (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Dong_CSWin_Transformer_A_General_Vision_Transformer_Backbone_With_Cross-Shaped_Windows_CVPR_2022_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/dong2022cswin.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT의 self-attention mechanism은 long-range depedencies를 모델링하는 데 좋은 성능을 보이며 여러 vision tasks에 적용되었지만 self-attention의 계산 비용은 매우 큼
- 이러한 연산량을 낮추기 위해 대다수의 연구는 고정된 local window 내로 attention region을 제한시키고 인접한 window를 연결하는 방식을 사용함
- 하지만 이러한 방법은 receptive field를 키우기 위해 오랜 과정이 필요하기 때문에 global self-attention의 목적을 달성하기 위해 많은 수의 block을 쌓아야 함
- 넓은 receptive field는 detection, segmentation을 포함한 여러 tasks에서 중요하기 때문에 large receptive field를 효과적으로 달성하면서 연산량을 낮은 수준으로 유지할 수 있는 방법이 필요함
- 따라서, 본 논문에서는 self-attention을 가로, 세로의 stripes 내에서 수행하는 Cross-Shaped Window(CSWin) self-attention을 제안함 
- 이러한 CSWin self-attention과 함께 hierarchical design을 사용하는 새로운 Transformer 아키텍처인 CSWin Transformer 구조 제안
- 추가로 detection 혹은 segmentation 같이 입력 이미지의 사이즈가 상이한 task에 잘 적용될 수 있는 효과적인 positional encoding 방법인 locally-enhanced positional encoding(LePE) 방법을 제안함

## 접근법
- CSWin Transformer는 hierarchical Transformer 구조를 사용함
> - Stage의 시작 부분에서 convolution을 통해 feature map의 size(sequence의 length)를 일정하게 줄이고 embedding dimension을 확장
- 또한, CSWin Transformer block은 기존의 self-attention module을 Cross-shaped Window Self-Attention으로 대체하였으며 LePE를 self-attention module과 병행하여 사용
- CSWin self-attention은 self-attention 연산을 일정한 가로, 세로의 stripes (horizontal, vertical stripes) 내에서만 수행함
- 이때 horizontal, vertical stripes attention을 순차적으로 수행하는 것이 아니라 self-attention의 multiple head를 두 그룹으로 나누어서 한 그룹에서는 horizontal을, 나머지 그룹에서는 vertical stripes attention을 수행하도록 함
- 두 그룹의 연산 결과는 최종적으로 concat되어 projection됨
- Stripes의 넓이를 초반 layer에서는 작게 설정하고 후반 layer에서 크게 설정하여 계산 비용을 최적화함
- Self-attention 이후 learnable parameter로 구성된 LePE를 value projection 전에 더해줌

## 실험결과
- Classification, detection, segmentation tasks에서 비슷한 복잡도와 모델 사이즈로 SOTA 성능 달성

## 의견
- /