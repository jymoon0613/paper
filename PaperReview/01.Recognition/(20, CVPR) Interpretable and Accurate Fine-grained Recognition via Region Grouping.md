# Interpretable and Accurate Fine-grained Recognition via Region Grouping (CVPR, 2020)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Huang_Interpretable_and_Accurate_Fine-grained_Recognition_via_Region_Grouping_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/huang2020interpretable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 모델의 해석가능성(interpretability)을 증진시킬 수 있는 방법 중 하나는 object를 의미있는 regions로 segment하고, 각각의 region이 decision에 어떻게 영향을 주는지를 식별하는 방법이 있음
- 하지만, 별도의 part annotations 없이 object parts를 학습하는 것은 매우 challenging함
- 따라서, 본 논문에서는 fine-grained recognition task에서 object parts를 찾고 각각이 recognition에 얼마나 중요한지를 측정하는 설명가능한 deep model을 설계하고자 함
- 주요 아이디어는 CNN에서 추출되는 피처를 시각적으로 응집되어 있는 regions로 그룹화하고, 나누어진 각각의 segments 일부를 선택하여 recognition을 위해 사용하는 것
- 또한, part annotations을 사용하는 대신 object의 part가 Beta distribution을 따른다는 간단한 prior을 사용함

## 접근법
- K개의 object parts가 있다고 가정
- 2D 공간상(${H}\times{W}$)에서 각 픽셀 위치가 k번째 object parts에 속할 확률을 계산하여 part assignment map($Q\in{R^{K\times{H}\times{W}}}$)을 만듦
- Part assignment map에 gaussian kernel을 적용하여 smoothing을 해주고, maxpooling을 적용하여 K개의 object parts의 occurence 확률을 각각 구함 (occurence vector $\in{R}^{K}$)
- Mini-batch 내의 N개의 samples에 대한 occurence vectors를 matrix로 변환하여 Beta distribution을 estimate하고, 사전에 정의한 prior Beta distribution과의 Wassertein distance를 계산하여 part occurence를 regularize함
- Feature map과 part assignment map을 통해 region features를 추출하고, 이를 바탕으로 region feature set($Z\in{R^{{D}\times{K}}}$)을 구함
- 이후 Z에 1x1 convolutions 및 softmax를 적용하여 attention($a\in{R^K}$)를 구함
- 이후 region features를 attention으로 re-weight하고, 최종 classification 진행 ($a$의 큰 값은 해당 region이 분류에 중요하다는 것을 의미)

## 실험결과
- ResNet-101 backbone 사용
- Classification 정확도와 함께 localization에서의 성능도 개선

## 의견
- /