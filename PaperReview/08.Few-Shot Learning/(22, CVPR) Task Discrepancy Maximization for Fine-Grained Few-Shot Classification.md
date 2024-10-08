# Task Discrepancy Maximization for Fine-Grained Few-Shot Classification (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Lee_Task_Discrepancy_Maximization_for_Fine-Grained_Few-Shot_Classification_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/lee2022task.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝은 엄청나게 발전하였지만, 딥러닝이 기대만큼의 성능을 내기 위해서는 수많은 labeled 데이터셋이 필요함 
- Labeled 데이터가 적다면 성능은 급격히 낮아짐 
- 대표적인 해결방법으로는 few-shot learning이 있음 
- Few-shot learning의 목적은 새로운 class에도 잘 적응하는 모델을 훈련시키는 것 
- 학습된 모델이 새로운 class에 대한 적절한 표현을 인코딩하는 것이 few-shot learning의 핵심 
- 이를 위해 주로 metric learning이 사용됨: 학습된 모델은 새로운 class들에 대한 representation으로 각 class의 cluster를 구성하고, 각 cluster의 center와 query의 representation 간의 거리를 기반으로 inference 
- 이러한 방식의 효과를 강화하기 위해 task-dynamic feature alignment strategies이 사용됨 
- 이 중 spatial alignment methods는 support set과 query set의 object location 차이로 인한 degradation을 해결하기 위해 제안됨 
- 반면 channel alignment methods는 feature map 수준의 변환을 통해 새로운 class를 더욱 잘 표현하도록 함 
- 이러한 방법들은 충분히 효과적이었지만, 이는 fine-grained image classification(FGIC)의 경우에는 부족함 
- FGIC의 경우에는 각 클래스를 구분할 수 있는 distinctive한 attributes들이 필요함 
- 따라서, 본 연구에서는 channel weight를 통해 object의 discriminatory details를 localize하는 few-shot 접근법을 제시함 
- 즉, 본 연구에서는 각 채널마다 채널에 weight를 주어서 discriminative한 regions를 localize하는 모듈인 Task Discrepancy Maximization (TDM)을 제안함 
- TDM은 discriminative한 regions를 표현하는 채널은 강조하고 나머지 불필요한 채널의 영향은 억제함 
- TDM은 Support Attention Module (SAM)과 Query Attention Module (QAM)으로 구성됨 
- SAM과 QAM은 각각 support set과 query set에 적용되어 각각의 채널 가중치를 도출함 
- 두 sub-module로부터의 가중치는 task-specific weight로 통합되며, 이는 최종적으로 task-adaptive feature maps를 구성하기 위해 사용됨 

## 접근법
- 먼저 feature extractor는 주어진 이미지로부터 feature map을 추출함 
- 각 클래스의 k개 support image들의 feature map의 평균이 prototype feature map으로 사용됨 
- Prototype feature map의 채널 방향 평균이 mean spatial features: 해당 클래스의 salient object regions를 표현 
- 한 클래스의 prototype feature map의 각 채널과 해당 클래스의 mean spatial features의 MAE를 계산하여 intra-class representatives score를 계산함: 각 채널이 해당 클래스에 대해 어느 정도의 대표성을 지니는지 계산 
- 한 클래스의 prototype feature map의 각 채널과 다른 클래스의 mean spatial features의 MAE를 계산하여 inter-class representatives score를 계산함: 각 채널이 다른 클레스와 구분되는 특징을 얼마나 보유하고 있는지 계산 
- SAM은 class prototype을 입력으로 받아 위의 두 스코어를 계산함 
- 두 스코어는 FC 레이어를 거쳐 SAM이 사용하는 가중치로 변환됨 
- 두 가중치는 weighted sum을 통해 support weight vector가됨 
- Support weight vector는 해당 클래스의 discriminative한 채널을 강조하고 클래스 간 공통적으로 공유하는 채널의 영향은 줄이는 역할을 함 
- QAM의 경우도 비슷하게 처리하되, query의 경우 클래스 정보를 알 수 없으므로 채널 간의 관계만을 반영함 
- Query 이미지 feature map의 각 채널과 채널 방향 평균이 적용된 query 이미지의 mean spatial feature 사이의 MAE를 계산하여 채널 수준의 대표성을 계산함 
- 계산된 score는 FC 레이어를 거쳐 query weight vector가 됨 
- Query weight vector는 모델이 query 이미지의 object-related 정보에 집중하도록 함 
- Support weight vector와 query weight vector는 합쳐져서 task-specific weight vector가 되며, support set의 이미지와 query 이미지의 feature map은 모두 task weight에 의해 task-specific한 feature map으로 변환됨 
- Refine된 support set 및 query image의 feature map을 통해 inference 수행 

## 실험결과
- TDM은 여러 few-shot system과 혼합되어 사용할 수 있다는 장점이 있음 
- 이를 위해 여러 few-shot system과 혼합하여 실험 진행 
- TDM을 같이 사용했을 때 거의 모든 datasets에서 성능 개선이 있었음 
- Ablation study도 진행함: sub-modules, pooling strategy, metric 

## 의견
- Discussion 및 limitation 섹션을 통해 제안하는 방법의 contribution을 강조하거나, 다른 방법과의 차이점을 강조하고, 한계점을 언급하는 것도 좋아보임 (연구의 신뢰도가 높아짐) 
