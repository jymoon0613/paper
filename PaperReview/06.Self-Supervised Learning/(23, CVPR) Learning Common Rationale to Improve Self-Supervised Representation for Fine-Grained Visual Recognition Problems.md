# Learning Common Rationale to Improve Self-Supervised Representation for Fine-Grained Visual Recognition Problems (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2303.01669)

<p align="center">
    <img width="400" alt='fig1' src="../img/shu2023learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Self-supervised learning(SSL)은 여러 vision tasks에서 효과적이었음이 입증되었으며, 그 중에서도 contrastive learning이 뛰어난 feature learning capability를 보여주었음
- 하지만 최근의 연구에서 contrastive learning이 'coarse-grained bias'를 지니고 있으며, 따라서 fine-grained한 features 학습에는 효과적이지 않을 수 있다는 점이 지적됨
> - 이는 매우 subtle한 features에 집중하여 시각적으로 거의 유사한 subcategory-level의 recognition을 수행하는 fine-grained visual recognition(FGVR)에서 contrastive learning이 효과적이지 않을 수 있다는 것을 의미
- 이러한 현상은 FGVR의 특징과 SSL의 learning objective에 의해 발생함
> - Contrastive learning은 same-instance feature 간의 거리(유사도)는 최소화하고, 반대로 different-instance 간의 거리는 최대화하도록 학습됨
> - 이 과정에서 모델은 loss minimization에 기여할 수 있다면 어떠한 visual patterns이던지 간에 그것을 학습할 것임
> - 하지만 FGVR에서 discriminative한 visual patterns는 매우 subtle하고 주로 key object parts에 집중되어 있음
> - 이는 다시 말해 SSL로부터 학습한 features의 많은 부분이 FGVR에서 유용하지 않을 수 있다는 것을 의미함
- 이러한 불일치를 완화하기 위해 일부는 사전 훈련된 object part detectors 혹은 saliency detectors를 사용하여 SSL이 일부 유효한 영역에먼 집중하도록 함
> - 하지만, 이러한 detectors는 결국 manual annotations에 의해 훈련되어야 한다는 점에서 SSL의 목적에 부합하지 않음
> - 또한, SSL의 성능이 detectors의 정확도에 크게 의존할 수 있음
- 이러한 문제를 해결하기 위해, 본 논문에서는 contrastive learning process 외에도 추가적인 screening mechanism을 학습하는 것을 제안함
> - Screening mechanism의 목적은 SSL에서는 유효하지만 FGVR과는 무관한 visual patterns를 제거하는 것
> - 이러한 screening mechanism은 GradCAM을 적용하여 간단하게 구현 가능함

## 접근법
- 제안하는 방법은 MoCov2 baseline에 GradCAM fitting branch(GFB)를 추가한 간단한 구조임
- SSL은 class-label을 사용하지 않으므로, GradCAM 계산 방식을 일부 수정하여 contrastive loss로부터 GradCAM을 계산함
> - 기존의 GradCAM(or GradCAM weights)은 각 region이 주어진 이미지를 주어진 클래스로 분류할 때 얼마만큼 기여하는지를 나타냄
> - 변경된 contrastive loss 기반의 GradCAM은 각 region이 주어진 이미지의 instance discrimination task에 얼마나 기여하는지를 나타냄
> - 이때 instance discrimination task에 기여하는 visual patterns가 항상 FGVR에서도 효과적인 것은 아님 (서론에서 언급)
- FGVR에 무관한 visual patterns를 필터링하기 위해 GFB를 추가함
- GFB는 backbone의 output feature map($\in{R^{{H}\times{W}\times{C}}}$)에 1x1 convolution을 적용하여 K개의 channel로 구성된 feature map을 생성하고($\in{R^{{H}\times{W}\times{K}}}$), max-operation을 적용하여 predicted GradCAM을 생성함($\in{R^{{H}\times{W}}}$)
- 이러한 GFB는 마치 K개의 object parts를 detect하는 과정으로 볼 수 있음
- Normalized된 ground-truth GradCAM과 predicted GradCAM 간의 KL divergence를 추가 loss로 사용함
- Inference 시에 GFB는 attention mask(= predicted GradCAM)를 생성하는 역할을 함
> - Attention mask는 backbone의 last conv feature map에 point-wise하게 곱해지고, 수정된 feature map은 average pooling이 수행됨
> - 즉 attention mask는 weighted average pooling을 위해 사용됨
> - 최종 feature는 downstream tasks를 위해 사용됨

## 실험결과
- Linear probing, image retrieval tasks에서 ResNet-50 baseline 및 MoCov2 baseline의 성능을 넘음
- BYOL, DINO를 포함한 다른 SSL 방법과의 비교에서도 가장 좋은 성능 달성

## 의견
- Image retrieval은 feature quality를 측정할 수 있는 실험
- FGVR 연구에서 제안하는 방법이 rich representation을 학습한다는 것을 증명하기 위해 image retrieval 성능을 측정
- 베이스라인과의 성능 비교를 통해 효과 증명