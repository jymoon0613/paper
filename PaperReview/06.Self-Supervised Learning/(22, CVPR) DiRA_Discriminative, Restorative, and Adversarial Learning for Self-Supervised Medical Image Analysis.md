# DiRA: Discriminative, Restorative, and Adversarial Learning for Self-Supervised Medical Image Analysis (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Haghighi_DiRA_Discriminative_Restorative_and_Adversarial_Learning_for_Self-Supervised_Medical_Image_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/haghighi2022dira.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 자기 지도 학습의 목적은 별도의 클래스 레이블 없이도 일반화 가능한 좋은 표현을 학습하는 것
- 이러한 자기 지도 학습은 크게 3가지로 카테고리화 할 수 있음: (1) Discriminative Learning: clustering 혹은 contrastive method를 사용하여 마치 pseudo label을 사용하는 것처럼 discriminative하게 학습하는 방법, (2) Restorative Learning: 생성 모델이 distorted image를 복원하도록 학습하는 방법, (3) Adversarial Learning: restorative learning을 강화하기 위해 adversary model을 활용하는 방법 
- 일반적인 비전 태스크에서는 discriminative 기반의 contrastive 방법이 성능이 매우 좋지만, medical image의 경우 restorative 기반의 방법도 유사하게 좋은 성능을 보여줌 
- 일반적인 비전 태스크에서는 분류를 위해 객체의 고수준 정보에 집중해야 하지만, medical image의 경우 고수준 정보도 중요한 반면 이미지 전체에 분산되어 있는 fine-grained한 details 정보도 매우 중요함
- 본 연구에서는 분석 과정을 통해 (1) discriminatice learning은 이지미의 고수준 정보 (global features)를 학습하는 데 효과가 좋았으며, (2) restorative learning은 이미지의 fine-grained한 details를 학습하는 데 효과가 있었고, (3) adversarial learning은 더 많은 details를 학습하도록 restorative learning을 강화한다는 것을 알아냄
- 따라서 본 연구에서는 이 세 방법을 모두 동시에 사용하여 클래스 레이블 없이 medical image의 상호 보완적인 피처들을 학습할 수 있는 자기 지도 학습 방법을 제안함 (DiRA)
- DiRA는 (1) medical image의 더 일반화된 특징들을 학습할 수 있고, (2) fully supervised ImageNet 성능을 능가함과 동시에 레이블이 부족한 상황에서 더 견고하며, (3) fine-grained한 특징들을 잘 잡아내어 이미지 수준 레이블만 가지고도 병변 위치 파악을 잘 할수 있고, (4) SOTA restorative 방법을 개선시켰음

## 접근법
### (1) Discriminative Learning
- Online network와 target network를 이용하여 discriminative learning 수행
- Augmented views에 대한 distance or similarity loss 계산을 통해 학습

### (2) Restorative Learning
- Fine-grained한 정보를 활용하여 discriminative learning 효과를 극대화하기 위해 사용
- Online network에서 distorted된 이미지를 복원하도록 훈련됨

### (3) Adversarial Learning
- 원본 이미지와 restorative learning에서 복원된 이미지를 구별하는 discriminator를 생성

### (4) Joint Training
- discriminative learning은 global한 특징들을 학습
- restorative learning은 fine-grained한 부분들을 학습
- adversarial learning은 restorative가 더욱 informative한 부분들을 학습하도록 강화
- 세 가지 훈련은 가중치와 함께 전체 손실 함수를 구성하고 동시에 수행됨
  
### (5)Implementation Details
- DiRA는 기존의 SSL 방법에 쉽게 결합될 수 있음
#### 2D Medical Image
- 2D 이미지에 대해서는 SimSiam, MoCo-v2, BarlowTwins와 결합해봄
- 각 방법의 similarity loss, projection head 구조를 그대로 사용함
- 백본은 ResNet-50 기반의 2D U-Net 구조를 사용함 (encoder(= online network & target network), decoder)

#### 3D Medical Image
- 3D 이미지에 대해서는 3D medical image의 SOTA SSL인 TransVW와 결합해봄
- 백본은 ResNet-50 기반의 3D U-Net 구조를 사용함 (encoder(= online network & target network), decoder)

## 실험결과
- 9개의 2D 및 3D medical image 데이터셋으로 검증
- DiRA는 기반이 되는 discriminative learning 방법의 성능을 개선
- DiRA는 클래스 레이블 제한적인 상황에서 강건성을 개선
- DiRA는 병변 위치 파악을 위한 weakly-supervised 세팅에서도 성능이 좋았음
- DiRA는 ImageNet에 사전학습된 ResNet-50 모델을 전이학습시켰을때, fully supervised한 방식으로 훈련시킨 ResNet-50 모델의 성능보다 뛰어났음
- 3D medical image에 대한 SSL 방법보다도 뛰어났음

## 의견
- Medical Image를 FGIR 평가에 사용해도 좋을 듯 (Fine-grained니까)
- 이 논문과 유사하게 기존의 SSL을 개선시켜서 실험하는 느낌도 괜찮을 듯 
> 기존 BYOL, MoCo 등등의 FGIR에서의 성능 vs. 내 방법을 적용했을 때의 성능
> 내가 제안하는 방법의 효과를 입증하는 데 사용
> 예를 들어, FaceMAE를 추가하는 것이 단순히 contrastive만 썼을 때보다 성능이 좋았던 것처럼
- 실험이 진짜 중요함 (왜 내가 제안하는 방법이 좋은지, 실제로 의도한대로 작동하는지)
- 왜 fine-grained한 details 학습에 좋은지는 검증을 못한 것 같음