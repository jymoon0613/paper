# Bootstrap Your Own Latent - A New Approach to Self-Supervised Learning (NIPS, 2020)

[논문 링크](https://proceedings.neurips.cc/paper/2020/hash/f3ada80d5c4ee70142b17b8192b2958e-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/grill2020bootstrap.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Downstream Task에 대한 효율적인 훈련을 위해 좋은 이미지 표현을 학습하는 것은 컴퓨터 비전에서 핵심적인 과제임 
- 이러한 좋은 이미지 표현 학습을 위해 여러 사전 훈련 과제 (Pretext Tasks) 기반의 훈련 방식이 제안됨 
- 이러한 사전 훈련 과제 중 대조 학습 방법 (Contrastive Learning Method)은 같은 원본 이미지에서 Augmented된 표현 간의 유사도는 증가시키고, 서로 다른 이미지로부터 Augmented된 표현 간의 거리는 멀어지도록 학습함 
- 하지만 지금까지의 대조 학습 방법은 Collapsed Representation을 방지하기 위해 Negative Pairs를 중요하게 고려해야 하며, 따라서, 많은 Negative Pairs를 고려하기 위한 큰 배치 사이즈, Memory Bank, Customized Mining Strategy가 필요함 
- 또한, 지금까지의 대조 학습 방법은 어떤 Augmentation을 적용하는가에 따라 성능이 크게 달라짐 
- 본 논문에서는 지금까지의 흐름과는 다른 대조 학습 방법인 Bootstrap Your Own Latent (BYOL)을 제안하며, BYOL은 Negative Samples를 사용하지 않음 
- BYOL은 네트워크의 출력을 반복적으로 Bootstrap하여 향상된 표현 학습을 위한 타겟으로 사용함 
(통계에서의 Bootstrap: 집단에서 샘플을 여러 번 무작위 복원 추출하는 과정을 통해 집단의 통계량을 추정) 
(ML에서의 Bootstrap: 데이터를 랜덤 복원 추출하여 무작위성을 줌으로써 Overfitting 방지 및 다양성 제고) 
(관용적인 표현인 Bootstrap: 도움을 받지 않고 스스로 상황을 개선시킴) 
- 또한 BYOL은 Data Augmentation 기법의 선택에 Robust한 성능을 지니며, 이는 BYOL이 Negative Paris를 사용하지 않기 때문으로 보임 
- Bootstrapping에 의존하는 여러 선행 연구가 있었지만, BYOL은 표현을 직접적으로 Bootstrapping한다는 차이가 있음 
- BYOL은 Online Networks와 Target Networks로 불리는 두 개의 네트워크로 구성되어 있으며 서로 상호작용하며 학습함
- Online Networks는 주어진 Augmented 이미지에 대해 같은 이미지로부터 생성된 다른 Augmented 이미지에 대한 Target Networks의 출력 표현을 예측하도록 훈련됨 
- 본 연구에서는, 1) Online Networks에 Predictor를 추가하는 것과 2) Online Networks 파라미터의 Moving Average로 Target Networks의 파라미터를 업데이트하는 것이 Online Networks가 더욱 더 유익한 정보를 인코딩하도록 장려하기 때문에 Collapsed Representation 현상을 방지할 수 있다고 주장함 
- 구체적인 Contributions은 다음과 같음: 1) BYOL이라는 새로운 Self-supervised Learning 방식을 제안했으며, 이는 Negative Pairs를 사용하지 않고도 ImageNet 데이터셋에서 Linear Evaluation Protocol의 SOTA 성능을 기록하였고, 2) BYOL은 Semi-supervised Learning과 Transfer Benchmarks에서도 SOTA 성능을 달성했으며, 3) BYOL은 다른 Contrastive Learning 모델에 비해 배치 사이즈 및 Augmentation 변화에 Robust함 

## 접근법
### (0) Related Work 
- 대부분의 Unsupervised 방식의 표현 학습 방법은 Generative한 방식과 Discriminative한 방식으로 구분 가능함 
- Generative한 방식은 주어진 데이터 및 Latent Embedding에 대한 분포를 구축하고 학습된 Embedding을 이미지 표현으로 사용함 
- Generative한 방식은 대표적으로 AutoEncoding을 이용하거나 Adversarial Learning을 이용함 
- Generative한 방식은 픽셀 공간에 직접 적용되기 때문에 계산량이 많고, Image Generation에 사용되는 높은 수준의 디테일은 표현 학습에는 불필요한 것일 수 있음 
- Discriminative한 학습 방식에는 대표적으로 Contrastive Learning이 있으며, Contrastive Learning 방식은 최근 Self-supervised Learning에서 SOTA 성능을 기록하면서 각광받음 
- Contrastive Learning은 같은 이미지로부터의 서로 다른 뷰를 가깝게 하고, 서로 다른 이미지의 뷰를 멀어지도록 하는 방식을 사용함으로써 Generative한 방식의 비용이 많이 드는 생성 단계를 제외시킴 
- 하지만, Contrastive Learning은 좋은 학습을 위해 비교할 여러 Negative Samples이 필요하다는 단점이 있음 
- DeepCluster 방법은 다음 Iteration의 표현 학습 Target을 생성하기 위해 이전에 출력된 표현을 Bootstrapping하는 방식을 사용하는 방식으로 Negative Samples 사용을 피함 
- DeepCluster는 이전의 출력 Representation으로부터 클러스터링을 진행하고, 클러스터 인덱스를 다음 Iteration의 Classification Target으로 사용함 
- 하지만, DeepCluster는 클러스터링을 위한 계산량이 많다는 문제가 있음 
- Patch Prediction, Jigsaw Puzzle와 같이 Handcrafted Prediction Task를 수행하는 다른 Discriminative한 학습 방식도 존재하지만, 이들은 모두 Contrastive Learning에 비해 성능이 좋지 못했음 
### (1) Overview 
- Contrastive Learning은 같은 이미지의 다양한 뷰에 대한 표현 유사도를 바탕으로 학습함 
- 하지만 이는 어떤 입력이 들어오던 간에 모델이 항상 같은 표현을 출력하는 Collapsed Representation 현상이 발생할 위험이 있음 
- 따라서 Contrastive Learning은 예측 문제를 판별 문제 중 하나로 재구성하여 이러한 문제를 해결하고자 하는데, 이는 같은 이미지의 다양한 뷰는 유사해지록 하되, 다른 이미지로부터의 뷰와는 달라지도록 학습하는 것임 
- 이러한 방식은 Collapsed Representation 현상을 방지하는 데 탁월하지만, 안정적인 학습을 위해서는 많은 Negative Samples가 필요하다는 단점이 존재함 
- BYOL은 Negative Samples를 사용하지 않고도 Collapsed Representation 발생을 피할 수 있는 훈련 방법 
- Collapsed Representation을 피하는 직관적인 방법은 랜덤하게 초기화된 네트워크가 예측의 타겟이 되는 표현을 생성하도록 하는 것 
- 이러한 방법은 Collapsed Representation을 피할 수 있지만, 성능이 좋을 것이라 기대할 수 없음 
- 하지만, 이러한 방식으로 학습한 네트워크는 초기화된 네트워크보다 좋은 성능을 보였으며, 이는 BYOL의 핵심 Motivation임 
- Target Networks로부터 생성된 Target 표현을 학습의 대상이 되는 Online Networks가 예측하도록 훈련함 
- 이러한 절차는 현재의 Online Networks가 이후 Iteration의 Target Networks가 되는 방식으로 반복되면서 진행됨 
- 이러한 과정을 통해 모델은 점차 표현의 질을 개선시킴 
- 대신 Online Networks에서 Target Networks의 전환은 Hard한 전환이 아닌 Online Networks의 Moving Exponential Average를 기반으로 Soft하게 진행됨 
### (2) Description of BYOL 
- BYOL의 궁극적 목적은 Downstream Tasks에 사용할 좋은 표현을 학습하는 것 
- BYOL은 Online Networks와 Target Networks 두 개로 구성됨 
- Online Networks는 Encoder Backbone, Projector, Predictor로 구성됨 
- Target Networks는 Online Networks에게 Prediction Target을 제공하며, Target Networks의 파라미터는 Online Networks 파라미터의 Exponential Moving Average임 
- Online Networks의 Prediction Representation과 Target Networks의 Target Representation의 값이 유사해지도록 학습함 (L2 Normalize, Mean Squared Error) 
- 이는 두 이미지 뷰에 대해 번갈아 진행되며, 즉, Loss는 두 부분으로 대칭을 이룸 
- 학습은 Online Networks로만 시킴, 즉, Target Networks는 Stop-gradient 사용 
### (3) Intuitions on BYOL’s Behavior 
- BYOL은 Collapsed Representation을 방지하기 위한 명시적인 트릭을 사용하지 않음 
- Target Network는 Loss로부터 Backpropagation하지 않으므로 (Stop-gradient) 정의된 Loss는 Online 및 Target Networks에 대해 Jointly Minimize되지 않음 (GAN의 Adversarial Loss와 유사) 
- 따라서, BYOL도 Collapsed Representation의 위험이 있지만, 아직 경험적으로 관찰된 바 없음 
- Optimal Predictor를 가정 
- 만약 Optimal한 Predictor가 항상 일정한 상수 c를 출력하도록 수렴한다면, 임의의 Random Variable 들의 Conditional Variance에 관한 부등식으로부터, Loss가 작거나 같은 값이 항상 존재하기 때문에 Constant로 수렴하는 경우가 적음 (불안정함) 

## 실험결과
### (1) Linear Evaluation on ImageNet 
- 다른 Self-supervised Learning 방법들의 성능을 능가 
- 더 깊은 네트워크 구조를 사용했을 때는 동일한 네트워크의 Supervised 모델의 성능을 능가 
### (2) Semi-supervised Training on ImageNet 
- 기존 데이터셋의 일부만 사용하여 End-to-End 학습 
- 다른 Self-supervised Learning 방법들의 성능을 능가 
### (3) Transfer to Other Classification Tasks 
- 모델이 학습한 표현이 ImageNet 데이터셋에 한정적인지, 일반화될 수 있는지를 검증 
- 여러 다른 데이터셋에서 SimCLR 및 ImageNet Supervised Pretraining Model을 능가 
### (4) Transfer to Other Vision Tasks 
- 모델이 학습한 표현이 이미지 분류 외의 여러 컴퓨터 비전 작업에도 유효한지를 검증 
- Semantic Segmentation, Object Detection, Depth Estimation에서 SimCLR 및 ImageNet Supervised Pretraining Model을 능가 
### (5) Ablation Study 
- SimCLR은 Negative Samples의 수가 학습에 매우 중요하기 때문에 배치 사이즈에 따라 성능이 크게 달라지는 것에 반해, BYOL은 Negative Samples를 사용하지 않으므로 Batch Size에 관계없이 모델의 성능이 일정함 
- SimCLR은 Color Distortion을 Augmentation에서 제거할 때 성능이 급격히 낮아짐 
- 이는 하나의 이미지는 어느정도 같은 Color Histograms을 공유하고, 다른 이미지는 Color Histograms 또한 다르므로, 모델은 Color Histograms의 정보만으로도 Loss를 최적화할 수 있게 되며 그 이외의 피처를 학습하지 못함 
- 반면 BYOL은 예측을 개선하기 위해 대상 표현에 의해 추출된 모든 정보를 온라인 네트워크에 유지하도록 장려되며, 따라서 Augmentation에 어느정도 Robust함 
- Momentum 파라미터에 따라 장단이 있음: 너무 빠르게 Target Network를 업데이트 시키면 학습이 불안정해지고 성능이 낮아지며, 너무 느리게 Target Networks를 업데이트 시키면 학습은 안정되지만 반복적인 개선을 방지하여 성능이 낮아짐 
- SimCLR의 경우, Negative Sample을 사용하지 않으면 (Beta = 0) 성능이 매우 나빠지지만, BYOL은 Negative Samples를 사용하지 않고도 좋은 성능 달성 (오히려 Negative Samples를 썼을 때 성능이 감소) 

## 의견
- SimCLR을 비롯한 Contrastive Learning 방법들이 지녔던 문제를 개선 
- 여러 측면에서 방법의 성능을 검증 

