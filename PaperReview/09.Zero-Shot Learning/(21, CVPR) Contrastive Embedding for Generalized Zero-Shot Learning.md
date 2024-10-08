# Contrastive Embedding for Generalized Zero-Shot Learning (CVPR, 2021)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2021/html/Han_Contrastive_Embedding_for_Generalized_Zero-Shot_Learning_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/han2021contrastive.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝의 발전에 따라 object recognition 성능은 눈에 띄게 발전해왔음
- 하지만, 일반적인 object recognition 연구에서 가정하는 상황과 달리 현실 세계의 object 카테고리는 long-tail distribution을 따르는 경우가 많음
- 즉, 일부 object 클래스에 대해서는 훈련 데이터가 풍부하지만, 일부 클래스에 대해서는 훈련 데이터가 매우 적거나 없을수 있음
- 이러한 long-tail 데이터셋에서의 object recognition은 클래스 불균형으로 인해 매우 어려움
- Zero-shot learning (ZSL)은 훈련 데이터에는 포함되지 않는 unseen 카테고리를 분류하는 것을 목표로 하는 학습 방법으로서 이러한 long-tail object recognition 문제를 해결할 가능성을 보여주었음
- ZSL은 우선 seen 데이터 (훈련 데이터에 포함되는 카테고리)에 대해서 훈련됨
- 이때 ZSL은 visual attributes나 word vectors와 같은 category-level semantic descriptors를 바탕으로 훈련하여 학습된 지식을 별도의 추가 훈련 없이 seen 데이터에서 unseen 데이터로 transfer하도록 함
- ZSL은 테스트 시에 unseen 데이터만 주어진다고 가정하지만, Generalized Zero-Shot Learning (GZSL)은 테스트 시에 unseen 데이터와 함께 seen 데이터도 주어진다고 가정함
- 대부분의 ZSL 접근법은 이미지의 visual features를 semantic descriptor space로 매핑시키는 것을 학습하고, 테스트 시에는 embedded data points를 주어진 class-level semantic descriptors와 직접 비교함으로써 unseen 데이터에 대한 분류를 수행하도록 함
- 이러한 방법은 일반적인 ZSL 가정에서는 잘 동작하지만, seen 데이터에 대한 bias도 고려해야 하는 GZSL에서는 성능이 매우 떨어지게 됨 (일반 ZSL에서는 테스트 시 seen 데이터는 고려하지 않으므로 seen 데이터에 대한 bias도 고려할 필요 없음)
- 이러한 bias 문제를 해결하기 위해 unseen 카테고리에 대한 가상 아마자룰 생성하여 추가하는 generation-based GZSL 방법이 고안되었음
- 이러한 방법은 seen 데이터와 가상의 unseen 데이터를 동시에 훈련시켜 fully-observed한 training set을 구성할 수 있다는 점에서 장점이 있음
- 하지만 feature generation 방법은 semantic space가 아닌 original feature space에서 이루어지므로 discriminative ability가 부족할 수 있다는 문제가 있음
- 따라서, 본 연구에서는 feature generation 방법과 semantic space의 장점을 결합하기 위해 feature generation model 위에 embedding model을 접목시킨 hybrid GZSL 프레임워크를 제안함
- 제안하는 프레임워크에서는 real seen 피처와 가상의 unseen 피처를 새로운 임베딩 공간에 매핑하고 최종 GZSL 분류를 임베딩 공간에서 수행함
- 임베딩 모델로는 contrastive embedding 방식을 사용함

## 접근법
- 우선 모델은 입력 이미지의 visual features를 추출하고, 이는 embedding function과 non-linear projection head를 통과함
- 결과적으로 최종 벡터는 K+1-way classification에 사용되며 cross-entropy를 통해 1개의 positive example(랜덤하게 선택된 입력 이미지와 같은 클래스 이미지의 최종 벡터)와는 가까워지면서 K개의 negative samples(랜덤하게 선택된 입력 이미지와 다른 클래스 이미지들의 최종 벡터)와는 멀어지도록 함
- 이는 embedding function이 강력한 discriminative 정보를 학습하면서 클래스의 공통적인 구조를 학습하도록 함 (Instance-level contrastive embedding)
- 다음으로, class-level contrastive embedding을 학습하기 위해서는 embedding function의 출력 벡터를 사용함
- embedding function의 출력 벡터와 같은 클래스의 semantic descriptor와는 가까워지고 다른 클래스의 semantic descriptor와는 멀어지도록 학습함
- Feature generation을 위한 loss는 그대로 사용

## 실험결과
- AWA1, AWA2, CUB, Oxford Flowers, SUN 데이터셋에서 평가

## 의견
- /