# Residual Attention Network for Image Classification (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Wang_Residual_Attention_Network_CVPR_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2017residual.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Attention은 입력 이미지의 집중해야 할 위치를 특정하는 역할을 할 뿐만 아니라 해당 위치에 존재하는 Object에 대한 다양한 표현을 촉진시킴 
- Attention Mechanism에 대한 활발한 연구가 진행되었지만, Image Classification에서의 SOTA 성능을 위해 CNN에 Attention Mechanism이 사용된 연구는 보고된 바 없으며, 최근의 Image Classification 연구는 네트워크의 깊이를 매우 깊게 설계하여 성능을 올리는 것에 집중함 
- 따라서, 본 연구에서는 Attention Mechanism과 Deep Neural Networks에서 영감을 받아 매우 깊은 CNN 구조에서 Attention Mechanism을 적용한 Residual Attention Network를 제안함 
- Residual Attention Network는 (1) Attention Mechanism이 주는 효과 이외에도 (2) Attention Module의 수를 늘릴수록 서로 다른 종류의 Attention 특징을 광범위하게 추출할 수 있으므로 일관된 성능 향상을 가져오며, (3) End-to-End 방식으로 깊은 SOTA 네트워크에 통합이 가능하며, 특히, 네트워크의 깊이를 수백 레이어까지 쉽게 확장 가능하다는 장점이 있음 
- 이러한 장점들은 Residual Attention Network가 (1) (Stacked Network Structure) Attention Modules을 쌓아 올린 구조로 되어 있어 각 모듈이 서로 다른 Attention 특징들을 잡아낼 수 있다는 점과, (2) (Attention Residual Learning) 단순히 Attention Modules을 쌓아 올리기만 한다면 명백히 성능이 감소할 것이므로 Attention Residual Learning Mechanism을 적용하여 매우 깊은 Residual Attention Network를 최적화시킨다는 점, (3) (Bottom-up Top-down) Bounding Box 및 기타 전처리에 의존하는 방식이 아니라 Bottom-up Top-down 구조를 통한 Feedforward 방식의 Soft Attention을 적용하여 End-to-End 학습이 가능하고 다른 네트워크에 적용이 쉽다는 점에서 기인함 
- (관련 연구) Attention은 인간의 인지 과정에서도 쉽게 찾아볼 수 있으며, 이러한 Attention의 개념은 Deep Neural Networks에서도 활발히 연구되고 적용되어 왔으며, 특히 RNN, LSTM과 같은 Sequence 데이터 처리 모델에 활발히 적용되고 있음. 이미지 분류에 적용되고 있는 Top-down 방식의 Attention Mechanism은 Sequential Process, Region Proposal, Control Gates 방식으로 나누어 볼 수 있음. Sequential Process는 RNN 혹은 LSTM을 적용하여 순차적으로 이미지의 Attention을 계산하고 처리함. Region Proposal은 이미지 피처 추출 전의 전처리 과정으로서 모델이 집중해야 할 영역에 Bounding Box를 그리는 방식으로 Attention을 진행함. Control Gates 방식은 각 레이어의 뉴런을 활성화/비활성화하는 방식으로 Attention을 진행함. 하지만, 최근의 이미지 분류 연구는 깊은 네트워크에 대한 개발에 집중되어 있으며, 이러한 깊은 CNN은 인간과 유사한 Bottom-up 방식으로 작동함. Residual Attention Network는 Human Pose Estimation과 같은 Task에서 사용되는 Bottom-up Top-down 구조를 사용하여 Soft Attention을 적용함. 

## 접근법
### (0) Overview 
- Residual Attention Network는 다수의 Attention Module이 쌓인 구조로 되어 있으며, 각 Attention Module은 Mask Branch와 Trunk Branch로 구성됨 
- Trunk Branch는 Convolution을 통한 피처 추출 기능을 수행함 
- Trunk Branch를 거친 Feature Map에 Bottom-up Top-down 방식으로 구한 Attention Mask를 적용하여 Soft Attention을 진행함 
- Attention Mask가 적용된 새로운 Feature Map은 마치 Control Gate 방식처럼 집중해야 할 부분이 강조되고, 이외의 부분은 무시됨 
- 또한 Attention Mask는 Backpropagation 과정에서 Gradient Update Filter로서 기능하여 Noisy Labels에 강건한 모델을 학습시킴 
- Attention을 Feedforward 과정에서 한 번만 사용하는 것은, 이미지가 매우 복잡할 때 여러 불필요한 영역을 세밀하게 제거하지 못할 뿐만 아니라 단 한 번의 Attention으로 중요한 영역을 찾아야 한다는 부담이 있고, 반면 여러 개의 Attention Module을 쌓는 것은 이미지의 다양한 부분을 Attention할 수 있을 뿐만 아니라 점차 Attention 과정을 개선하면서 처리가 가능함 (두 문제 해결) 
### (1) Attention Residual Learning 
- Attention Module을 여러 층 쌓는 것은 0에서 1사이의 값을 갖는 Attention Mask를 순차적으로 곱한다는 의미이고, 이는 (1) Feature Map의 값을 Degrade할 수 있으며 (2) 너무 많은 요소 제거로 Trunk Branch에서 추출한 의미 있는 정보까지 무시해버릴 수 있음 
- 따라서, Residual Learning을 사용하여 Attention Mask를 적용한 후 Attention Mask를 적용하기 전 Feature Map을 더함 (Attention Residual Learning) 
- 이러한 Attention Residual Learning은 Attention Module을 다층으로 쌓을 때 나타나는 문제들을 해결할 수 있으며, ResNet의 경우처럼 Attention Layer를 매우 많이 쌓아도 일관적인 성능 향상을 기대할 수 있도록 함 
### (2) Soft Mask Branch 
- Mask Branch에서는 Bottom-up Top-down 구조로 Attention Mask를 생성함 (Hourglass Structure) 
- Bottom-up 단계에서는 이미지의 Resolution을 축소시키면서 이미지의 Global한 정보를 압축함 
- Top-down 단계에서는 이미지의 Global한 정보에 따라 원본 크기로 이미지를 재구성함 
- 이후 1x1 Convolution과 Sigmoid를 통해 Attention Mask를 생성함 
- 또한, Bottom-up과 Top-down 단계를 연결하는 Skip Connection을 사용하여 서로 다른 Scale에서의 이미지 정보를 보존함 
### (3) Spatial Attention and Channel Attention 
- Spatial Attention과 Channel Attention을 구분하지 않고 동시에 적용하는 Mixed Attention과, Spatial Attention, Channel Attention 간의 성능을 평가한 결과 Mixed Attention이 더 좋았음 
- 즉, 이전의 연구처럼 Channel, Spatial, Scale 등의 Attention 대상에 제약을 두지 않고 Mixed Attention을 사용하여 전체적으로 적용하는 것이 더욱 효과적임 

## 실험결과
### (1) Attention Residual Learning 
- Attention Residual Learning을 사용하는 경우와 사용하지 않는 경우를 비교한 결과, ResNet의 결과처럼 Residual Learning을 사용하지 않는 경우 네트워크의 깊이가 매우 깊어지면 성능이 오히려 감소하는 것에 반해, Residual Learning을 사용하는 경우 네트워크 깊이에 따라 성능이 일관적으로 증가함 
### (2) Comparison of Different Mask Structures 
- 논문에서 제시하는 Encoder-Decoder 구조 (Bottom-up Top-down, Hourglass)의 Mask 생성 방식과 다른 Attention Mask 생성 방식 (Local Convolution, Convolution 연산을 통해 Attention Mask를 계산하는 방법)을 비교 평가함
- 본 논문의 방식이 더욱 성능이 좋았음 
### (3) Noisy Label Robustness 
- Attention Mask는 Backpropagation 과정에서 Gradient Update Filter로서 기능하여 Noisy Labels에 강건한 모델을 학습시킨다는 점을 확인하기 위해 Noise Robustness를 측정함 
- 제안된 모델은 ResNet164 모델과 비교했을 때 동일한 Noise Level에서 에러율이 모두 낮았으며, Noise Level이 증가할수록 에러율이 증가하기는 하지만 그 증가 폭이 ResNet164 모델보다 작음 
- 이는 Residual Attention Network가 높은 수준의 Noise Data에서도 잘 동작한다는 것을 의미하며, Label이 Noisy할 때 Attention Mask가 Noisy한 Label에서의 Gradient를 정제하는 역할을 할 수 있다는 것을 의미함 
### (4) Comparison with SOTA Methods 
- CIFAR-10, CIFAR-100 모두에서 SOTA 성능을 기록함 
- 또한, 기존 SOTA 모델보다 적은 네트워크 깊이 (적은 파라미터 수)로 좋은 성능을 기록하여, 제안하는 Attention 방식이 매우 효과적임을 입증 
### (5) ImageNet Classification 
- ImageNet 데이터셋에서도 더 적은 파라미터 수로 SOTA 성능을 기록함 
- ResNeXt, Inception과 같은 Basic Unit에도 적용하여 해당 Attention Mechanism이 여러 네트워크와 쉽게 결합될 수 있음을 보여줌 

## 의견
- BAM, CBAM과 같은 Attention Module의 Baseline 