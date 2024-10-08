# Patch Slimming for Efficient Vision Transformers (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Tang_Patch_Slimming_for_Efficient_Vision_Transformers_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/tang2022patch.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 좋은 성능만큼이나 많은 연산량이 요구됨
- 하지만 CNN과 달리 ViT에서의 경량화를 위한 논의는 크게 진행되지 않음
- 따라서, 본 논문에서는 ViT의 경량화를 위해 불필요한 이미지 패치를 식별하고 제거하는 patch slimming 알고리즘을 제안함

## 접근법
- 저자들의 실험에서 ViT의 레이어가 깊어질수록 패치들 간의 유사도가 높아지며 이는 깊은 레이어로 갈수록 패치들이 redundant하다는 것을 의미함
- 이러한 redundant한 패치들을 제거하더라도 성능에 큰 영향이 없을 것
- Patch slimming이 redundant한 패치들을 식별하고 제거하기 위해 사용됨
- Multi-head self-attentin(MSA) 및 MLP에서 각 패치별 유지여부를 결정하는 binary vector를 사용하여 slimming 진행
> - Query 계산을 포함한 연속된 여러 연산이 선택된 패치들에 대해서만 이루어지므로 연산량을 크게 줄일 수 있음 (patch pruning)
- ViT의 각 레이어별로 patch slimming을 수행하기 위해 top-down 방식을 사용함
> - CNN에서는 주로 channel pruning을 사용하며, 이 때 각 채널은 fully-connected되어 있고 전체 이미지에 대한 정보를 내포하고 있으므로 각 레이어 별로 독립적으로 pruning을 진행해도 상관없지만, ViT의 경우 각 패치는 이미지의 고유한 부분을 표현하고 있으며, 이들의 관계는 attention map을 통해 정의되므로 CNN과 같이 독립적인 방식을 사용할 수 없음
> - ViT의 최종 block에서 먼저 패치들을 선택하고 앞 레이어로 선택 과정을 진행함
> - 깊은 레이어에서 선택된 패치들은 하위 레이어에서도 계속 유지되도록 함 (하위 레이어는 상위 레이어보다 항상 많거나 같은 수의 패치를 선택)
- Patch slimming의 목적은 최대한 적은 패치들을 선택하면서 최대한의 정보를 유지하는 것이므로 이에 적합한 objective function을 정의
> - 각 레이어의 선택되는 패치들을 최소로 하되, 선택된 패치들의 representation과 기존 패치들의 representation 간의 차이가 최소가 되도록 함

## 실험결과
- 다른 경량화 기법보다 성능이 뛰어났음
- SOTA 모델과 비교했을 때 FLOPs가 크게 감소하는 데 반해 정확도는 아주 조금만 하락함

## 의견
- /