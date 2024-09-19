# Global and Local Mixture Consistency Cumulative Learning for Long-tailed Visual Recognitions (CVPR, 2023)

[논문링크](https://www.researchgate.net/publication/368956093_Global_and_Local_Mixture_Consistency_Cumulative_Learning_for_Long-tailed_Visual_Recognitions)

<p align="center">
    <img width="800" alt='fig1' src="../img/du2023global.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 현실 세계의 데이터는 주로 long-tail distribution을 따르는 경우가 많음
> - 소수의 class가 거의 대부분의 샘플을 차지함 (head classes)
> - 대부분의 다른 class는 매우 적은 샘플을 차지함 (tail classes)
- Tail classes는 medical diagnosis나 autonomous driving과 같은 분야에서 매우 중요함
- 하지만, long-tailed datasets에서 바로 학습을 하게 되면 모델의 예측이 head classes로 과하게 편향되는 문제가 발생함
- Long-tailed distribution을 rebalanced 시키기 위한 클래식한 전략으로는 training data를 resampling하는 방법과 cost-sensitive한 reweighting loss function을 디자인하는 방법이 있음
> - Resampling: mini-batch에서 tail class 샘플을 오버샘플링하거나 head class 샘플을 언더샘플링함
> - Reweighting: loss 계산 시 tail class에 가중치를 줌
> - 하지만 이러한 직접적인 rebalance 방법들은 기존의 분포를 훼손한다는 문제가 있음 
> - 또한, 모델은 오히려 tail classes에 오버피팅 될 수 있고 head classes의 성능이 크게 낮아질 수 있음
> - 따라서, 이러한 클래식한 방법들은 주로 2-stage 학습 전략을 사용하는데, 이는 feature extractor는 기존의 dataset에서 학습시키고, classifier만 balance된 환경에서 fine-tuning함
> - 하지만, 이는 training 시 고려해야하는 사항을 증가시키고, training의 overhead를 증가시킴
- 본 논문에서는 robustness를 향상시키면서 head classes에 대한 편향을 완화하는 간단한 long-tail recognition 학습 패러다임을 제안함
> - training 고려사항과 overhead를 최소화
> - Representation의 robustness를 향상시키기 위해 contrastive learning 기반의 auxiliary loss를 정의
> - Head class bias 문제를 해결하기 위해 기존 보다 개선된 reweighted loss를 정의
> - Global and Local Mixture Consistency Cumulative Learning Framework (GLMC)

## 접근법
- 딥러닝 모델은 피처 추출을 담당하는 encoder와 최종 분류를 수행하는 linear classifier로 나눌 수 있음
- Encoder의 generalization ability를 향상시키는 것은 long-tailed challenge에서의 분류 정확도 향상에 큰 기여를 할 수 있음
- 따라서, 본 연구에서는 encoder를 standard supervised task와 self-supervised task 두 가지를 사용하는 multi-task 방식으로 학습시킴
- Self-supervised task에서는 MixUp과 CutMix를 사용하여 global-local consistency를 학습하고자 함
> - MixUp은 global mixture를 위해 사용되며 CutMix는 local mixture를 위해 사용됨
> - 두 방법으로 augmented된 이미지의 negative cosine similarity를 최소화하도록 훈련됨 
- 또한 생성된 두 augmented views를 사용하여 supervised loss(negative cross entropy)도 계산함
- 이때 두 원본 이미지의 class weights을 weighted sum하여 supervised loss의 weight로 사용함 (rebalanced loss)
> - Minor class 학습을 원활히 하기 위해
- 또한, rebalanced loss를 사용하지 않는 supervised loss도 사용하되 supervised loss와 rebalanced loss를 적절히 weighted sum하여 같이 사용함
> - 이때 훈련이 진행되면서 supervised loss의 영향력이 점점 줄어들도록 weight를 변경함

## 실험결과
- CIFAR10-LT, CIFAR100-LR, ImageNet-LT로 모델을 평가함
- Long-tailed recognition task에서 SOTA 성능 달성함

## 의견
- /