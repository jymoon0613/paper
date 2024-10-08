# Fine-Grained Object Classification via Self-Supervised Pose Alignment (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Yang_Fine-Grained_Object_Classification_via_Self-Supervised_Pose_Alignment_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2022fine.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이미지 기반 객체 분류에서 semantic pattern은 객체 클래스의 시각적 모양에 의해 결정됨
- 하지만 FGIR의 경우 클래스 간 차이가 매우 미세하므로 semantic pattern 인식이 어렵고 분류가 어려움
- 따라서, Fine-Grained Image Recognition(FGIR)의 경우 local parts를 탐지하는 것이 중요하며 이를 위해 expensive한 part annotations이 요구되기도 함 
- FGIR은 여전히 어려운 문제이며 FGIR에서 좋은 표현이란 변형에 강건하고 미세한 차이를 잘 표현할 수 있어야 함
- 이를 위해, 대부분의 선행 연구에서는 전역적 피처 (global features)를 보완하기 위해 local regions의 fine-grained details을 추출하는 것에 집중했으며, 이외에도 명시적으로 객체를 localizing하지 않고 feature 벡터에 대한 bilinear pooling을 적용하여 객체 표현의 discrimination을 증진시키려는 시도도 있었음
- 하지만, 이들 중 오직 일부만 객체의 포즈와 각도 등의 변화에 따른 피처의 불일치 문제를 다루었음
- 다른 포즈 추정 문제와 유사하게, 객체의 포즈는 이미지에 분산되어 있는 local parts의 기하학적인 구성으로 설명될 수 있음 
- 따라서, 본 연구에서는 신뢰할 수 있는 객체 parts localization과 parts alignment가 포즈 변형에 민감하지 않은 객체 표현을 생성하는 것에 매우 중요하다고 주장함 
- Labeled 데이터 부족 문제를 고려하여, 본 논문에서는 포즈와 parts 정보에 대한 annotation 없이 unsupervised하게 객체의 part details와 포즈를 추정하는 Part-to-Pose Network (P2P-Net)을 제안함 
- P2P-Net은 weakly supervised manner로 region proposals로부터 class-discriminative regions를 찾아냄 
- Contrastive method를 통해 global features에 local part features가 반영되도록 함 
- 기존의 part-based methods와 다른 점은 parts 정보를 self-supervised한 방식으로 찾고 align함으로써 포즈에 의한 변동성을 감소시킴 
- Part branch는 학습 시에만 사용되어 inference 시의 계산량이 적음

## 접근법
### (1) Curriculum Supervision on Backbone Network

- ResNet 기반의 CNN 모델이 백본으로 사용됨
- 백본의 각 intermediate layer로부터 피처맵을 추출함
- 추출된 피처맵은 MLP을 거쳐 분류에 사용됨 (각각의 피처맵으로 분류 + 모든 피처맵을 결합하여 분류)
- 네트워크가 깊어질수록 분류 정확도가 높아진다는 점을 바탕으로, label smoothing 기법을 사용한 curriculum training 전략을 사용함
- 최종 분류기와 가까운 레이어에서 추출된 피처 벡터일수록 더 confident한 레이블로 학습함

### (2) Contrastive Feature Regularization
- 본 연구에서는 object detection의 관점에서 local details를 찾으며, 추출된 local datails는 global representation와 결합되어 하나의 image representation으로 표현됨
- 백본의 마지막 convolution block은 Feature Pyramid Network (FPN)에 연결됨
- FPN을 통해 local parts 영역을 추출
- 추출된 영역을 기반으로 이미지를 crop하고, crop된 이미지는 resize 후 백본으로 들어감
- 원본 이미지 분류 과정과 같은 과정을 거쳐 classification 진행 (curriculum training 전략도 동일)
- classification loss가 작을수록 local parts일 confidence가 높다는 가정에 의해 ranking loss를 적용
- ranking loss를 통해 임의의 part가 어떤 part보다 classification loss가 낮다면, FPN에서도 보다 높은 score를 받도록 학습됨
- Contrastive method를 통해 각 레이어에서의 원본 이미지 피처와 각 part 피처 분포가 유사해지도록 학습함 (학습된 특징을 공유함, KL-divergence)
  
### (3) Graph Matching for Part Alignment
- Local parts 추출은 weakly-supervised한 방식으로 이루어지므로 (별도의 part annotation 없이 class label에 따른 classification으로만 학습) 실제로 중요한 local parts (머리, 몸통, 꼬리 등)이 완벽히 추출되지는 못함
- 또한 FPN으로부터 상위 N개의 parts를 추출하므로, 피처가 추출되는 순서는 무작위하며 이는 global features와의 통합 과정에서 불일치를 유발함
- 이를 해결하기 위해 unsupervised graph matching 방법을 사용함
- 서로 다른 이미지에서 추출된 local parts라도 같은 local parts를 표현하고 있으면 correlation이 높을 것이라는 가정
- Reference correlation matrix로부터 최적의 순서를 결정
- Reference matrix는 점차적으로 update됨

### (4) Training and Inference
- 학습은 4개의 loss (global classification, part classification, rank loss, contrastive loss) 최적화를 통해 이루어짐
- Inference 시에는 원본 이미지에 대한 curriculum output의 sum으로 classification 진행

## 실험결과
- FGIR의 벤치마크 데이터셋인 CUB-200-2011, Stanford Cars, FGVC-Aircraft 데이터셋에서 평가 진행
- 학습에는 다른 annotation 없이 오직 class label만 사용
- 기존의 SOTA 모델들의 성능을 개선함

## 의견
- FPN 사용하는 방식 SimFLE에 적용할 수 있을 듯
- 우리의 경우도 눈, 코, 입 등 사실상 parts 정보는 제한적임
- Graph Matching 적용하면?
- ablation에서 각 모듈을 정당화하는 설명이 매우 좋음 (배울점)
- RMSE 이용해서 클래스별 representation의 centerness 평가하는 방법 매우 좋음 (좋은 피처 학습의 근거)

[논문 링크]: https://openaccess.thecvf.com/content/CVPR2022/html/Yang_Fine-Grained_Object_Classification_via_Self-Supervised_Pose_Alignment_CVPR_2022_paper.html