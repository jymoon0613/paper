# Looking for the Devil in the Details: Learning Trilinear Attention Sampling Network for Fine-Grained Image Recognition (CVPR, 2019)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2019/html/Zheng_Looking_for_the_Devil_in_the_Details_Learning_Trilinear_Attention_CVPR_2019_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zheng2019looking.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-grained Classification은 일반적인 이미지 분류 문제보다 클래스 간 구분이 가능한 디테일이 매우 작기 때문에 Challenging한 Task임 
- 이러한 Fine-grained Classification에 대해 Attention/Part 기반의 방식들은 지역적 피처 감지기를 학습시키거나, Attention이 필요한 부분을 Crop하거나, 여러 지역적 피처들을 연결하는 방식을 사용하여 문제를 해결하려 함 
- 하지만, 이러한 Attention/Part 기반의 방식은 1) Attention의 수가 제한되고 사전에 정의되어야 한다는 점에서 유연성과 효율성이 제한적이고, 2) Local Feature 추출을 위한 추가적인 Annotation Label이 없으면 일관된 Attention Map을 학습하기 어려우며, 여러 부분으로 나뉘어져 학습되는 CNN 학습은 효율적이지 못하다는 한계가 존재함 
- 따라서, 본 연구에서는 위의 문제를 해결하기 위해 Trilinear Attention Sampling Network를 제안하여 수백개의 Part Proposals로부터 Fine-grained Details를 학습하고, 학습된 피처들을 효과적으로 하나의 CNN으로 투입하고자 함 

## 접근법
### (1)  Details Localization by Trilinear Attention 
- 주어진 이미지를 ResNet18 Backbone Network를 사용하여 Feature Map으로 추출 (높은 Resolution 유지를 위해 원본 Network에서 Convolutional Stride를 변경하여 Downsampling 과정을 제거) 
- Feature Map의 Dimension을 2차원으로 변환하고, Outer Product를 진행하여 채널 간의 Spatial Relationship을 계산하고 Softmax 적용 (Bilinear Feature)
- 이후 다시 2차원으로 변환된 Feature Map과 Inner Product하여 Spatial Relationship과 원본 Feature Map을 다시 결합하고 Softmax 적용 (Trilinear Attention Map) 
- Trilinear Attention Map을 다시 원래의 차원으로 돌림 
### (2)  Details Extraction by Attention Sampling 
- 원본 이미지와 Trilinear Attention Map을 사용하여 Structure-preserved Image와 Detail-preserved Image를 생성하는 과정 
- Structure-preserved Image는 원본 이미지와 비교했을 때 디테일이 포함되지 않은 영역을 제거하여, Discriminative한 영역을 고해상도로 더 잘 표현할 수 있음 
- Detail-preserved Image는 하나의 부분에 초점을 맞추므로 보다 세밀하게 디테일이 보존된 이미지를 표현할 수 있음 
- Structure-preserved Image는 Attention Map에 Channel 방향으로의 Average Pooling을 적용한 뒤, 원본 이미지와 Point-wise Multiplication하여 구함 
- Detail-preserved Image는 Attention Map의 여러 채널 중, 하나의 Channel을 선택하여 원본 이미지와 Point-wise Multiplication하여 구함 
- Detail-preserved Image를 도출하는 과정은 Random한 선택이 이루어지게 되고, 따라서 학습이 진행되면 될수록 모든 Attention Map이 선택될 수 있고 다양한 세부 정보를 Asynchronously하게 Refine할 수 있음 
- 확률론적 관점에서 Sampling 의미 설명함 
### (3)  Details Optimization by Knowledge Distilling 
- Structure-preserved Image와 Detail-preserved Image를 같은 CNN으로 (ResNet50) 각각 하나의 벡터로 추출 
- 이후 각 벡터에 Softmax를 적용하여 각 Class에 대한 확률 분포로 변환 (Temperature = 10)
- Structure-preserved Image의 처리를 담당하는 Master-net의 Softmax 벡터를 사용하여 Classification Error를 측정
- 이후 Master-net의 Softmax 벡터와 Detail-preserved Image의 처리를 담당하는 Part-net의 Softmax 벡터를 이용하여 둘 사이의 Soft Target Cross-entropy를 계산 
- Master-net의 학습을 위한 최종적인 Loss Function은 Classification Error에 가중치가 곱해진 Soft Target Cross-entropy를 더하여 계산 (가중치 = 2) 
- Soft Target Cross-entropy는 Part-net에서 세밀한 디테일을 위한 학습된 피처들을 Master-net에 효과적으로 전달하는 것을 목표로 함 
- Attention-based Sampler가 각 반복마다 무작위로 Detailed-preserved Image를 추출하도록 하기 때문에 모든 세밀한 디테일이 훈련 과정에서 Master-net으로 전달될 수 있음 
- Master-net과 Part-net은 같은 CNN 구조를 지니며 (ResNet50) 파라미터를 공유 
- 즉, Master-net과 Part-net은 Teacher – Student의 구조를 지님 

## 실험결과
- CUB-200-2011, Stanford Cars iNaturalist와 같은 Fine-grained 이미지 분류 데이터셋에서 평가함 
- 여러 Attention Mechanism 방법과 비교해보았을 때, 논문에서 제시하는 Trilinear Attention 방식이 가장 성능이 좋았음 
- Attention-based Sampler의 성능을 측정하기 위해 여러 샘플링 기법들과 비교해본 결과, Attention-based Sampling을 사용하는 경우 성능이 가장 좋았으며, 또한 Master-net과 함께 Part-net을 같이 사용하는 경우 성능이 제일 좋았음 
- Knowledge Distilling 기법을 사용하는 경우 성능이 제일 좋았음 

## 의견
- Trilinear하게 Attention Map을 추출하는 방식은 참고할 만함 
- Attention Map 샘플링 기법 참고? 
- SOTA 성능 달성을 하지 못하더라도 비슷한 접근법을 쓴 방법론보다 낫다는 것을 보여주면 가능 
(ex. FER에 SSL을 사용한 논문과의 성능 비교) 
- 혹은 나의 접근법 및 방법론이 가지는 의의를 강조하여 논문 쓰기 
- 위의 경우에는 방법이 명확한 문제점 도출이 필요하며, 접근법이 참신해야 하고, 문제 해결 과정이 그럴듯함과 동시에 실험 과정과 결과에서 이러한 점이 명확히 드러나야 함 

