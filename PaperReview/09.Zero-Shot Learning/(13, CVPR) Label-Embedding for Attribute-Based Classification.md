# Label-Embedding for Attribute-Based Classification (CVPR, 2013)

[논문링크](https://www.cv-foundation.org/openaccess/content_cvpr_2013/html/Akata_Label-Embedding_for_Attribute-Based_2013_CVPR_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/akata2013label.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Zero-shot learning의 가장 대표적인 솔루션은 attribute layer라고 불리는 intermediate space $\mathcal{A}$를 도입하는 것임
> - Attributes는 여러 classes에서 공유되는 objects의 high-level properties를 의미함
- 기존의 attribute-based prediction algorithm은 한 attribute 당 하나의 classifier가 필요했음
> - Training set에 없는 이미지를 분류하기 위해, 먼저 학습된 attribute classifiers를 사용하여 주어진 이미지로부터 각 attribute 별 score를 예측함
> - 예측된 attribute 별 score와 가장 유사한 attribute description을 지니는 class로 입력 이미지를 분류함
> - 이러한 two-step 전략을 direct attribute prediction(DAP)라고 부름
- 하지만 DAP는 몇 가지 문제를 수반함
> - (i) Attribute classifiers는 attribute prediction task를 통해 사전에 pre-train되어야 하며(intermediate problem), 이는 궁극적 task인 class prediction과의 간극을 유발할 수 있음
> - (ii) DAP는 zero-shot learning에 탁월하지만, 새로운 training samples가 주어지면 점진적으로 이를 모델에 반영하는 incremental learning scenario에는 적합하지 않음
> - (iii) 또한, attributes 외에도 WordNet의 semantic hierarchies 같은 추가적인 정보를 zero-shot learning에 활용할 수 있지만, DAP는 이러한 추가적인 sources를 반영하기 어려움
- 본 논문에서는 label embedding framework를 사용하여 위의 문제들을 해결하고자 함
> - 각 class label을 attribute vectors로 embedding함 (attribute label embedding, ALE)

## 접근법
- 먼저 input image $x$와 그에 대응되는 label $y$사이의 compatibility를 측정하는 compatibility function $F$를 정의함
- 최종적인 prediction function $f$는 input image $x$에 대해 $F(x,y)$가 최대가 되는 label $y$로 예측을 수행함
- 이때 $x$와 $y$sms embedding function $\theta$와 $\varphi$에 의해 image embedding $\theta(x)$와 label embedding $\varphi(y)$로 projection됨
> - Compatibility function $F$는 $\psi(x)$와 $\psi(y)$의 L2-norm 등으로 정의할 수 있음
- 이때 본 논문은 label embedding function $\varphi$을 attributes를 반영하여 계산하는 $\varphi^{\mathcal{A}}$로 정의함
- $\varphi^{\mathcal{A}}(y)$는 class label $y$와 attribute 사이의 association을 계산하는 association measure을 사용하여 $y$와 전체 $E$개의 attributes 사이의 association $\rho_{y,i}$를 계산함
> - $\varphi^{\mathcal{A}}(y) = [\rho_{y,1}, \dots, \rho_{y,E}]$는 $E$ 차원의 vector가 됨
- 또한, attributes를 사용하는 label embedding 대신 다른 source도 사용할 수 있음

## 실험결과
- Zero-shot learning scenario에서 기존의 DAP보다 좋은 성능을 기록함

## 의견
- / 