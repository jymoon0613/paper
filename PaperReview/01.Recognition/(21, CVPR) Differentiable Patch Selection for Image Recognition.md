# Differentiable Patch Selection for Image Recognition (CVPR, 2021)

[논문링크](https://openaccess.thecvf.com/content/CVPR2021/html/Cordonnier_Differentiable_Patch_Selection_for_Image_Recognition_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/cordonnier2021differentiable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Sensor와 consumer devices의 발전은 high-resolution 이미지에 대한 처리 방법을 요구함
- 하지만 high-resolution 이미지 자체를 바로 처리하게 되면 연산량이 매우 커지며, 단순히 downsampling을 할 경우 중요한 fine details를 잃어버린다는 문제가 있음
- 이를 위해 다른 vision tasks에서도 많이 쓰는 것처럼 이미지의 모든 부분이 중요한 것은 아니라는 가정을 바탕으로 이미지 내의 중요한 regions을 patch 단위로 추출하는 방법이 있음
> - 불필요한 영역 제거는 모델의 연산량을 감소시킴
> - 핵심적인 영역을 기반으로 학습한 모델은 성능이 더 좋음
- 하지만, 이미지의 어떤 부분이 추출되어야 하는지를 결정하는 것은 매우 어렵고 task-specific함
> - 단순 center crop은 모든 task에 적합하지 않음
> - 이미지를 동일한 크기의 이미지 패치로 구분하고 처리할 패치를 결정하는 것은 end-to-end 학습에 적합하지 않음
- 따라서, 본 논문에서는 end-to-end로 학습이 가능한 patch selection method를 제안하고자 함
> - Patch selection 문제를 ranking problem으로 formulate함
> - 작은 CNN이 패치별 relevance scores를 예측하고 Top-K score 패치들이 downstream processing을 위해 선택됨

## 접근법
- 제안하는 모델은 scorer network, patch selection module, feature network, aggregation network로 구성됨
> - Scorer netwokrk: 패치들의 score를 계산
> - Patch selection module: 패치들의 일부를 선택
> - Feature network: 각 패치의 임베딩을 계산
> - Aggregation network: 계산된 임베딩을 combine하고 최종 예측 수행
- Scorer network는 down sampled된 이미지로부터 주어진 task에 대해 이미지의 각 region이 얼마나 관련되어 있는지 relevance score를 계산
- Patch selection module은 score를 바탕으로 K개의 image patch를 선택함
> - 총 hxw개의 패치가 있다고 가정
> - Score가 주어졌을 때 Top-K는 K개의 가장 salient한 패치들의 indices를 반환함
> - perturbed maximum method를 사용하여 Top-K를 미분가능하게 한 것이 main contribution
- 선택된 K개의 이미지 패치는 concat되어 feature network를 통과함
- Aggregation network는 최종 output을 산출함

## 실험결과
- Traffic sign recognition, CUB-200, inter-patch reasoning tasks에서 좋은 성능을 보임

## 의견
- /