# RIFormer: Keep Your Vision Backbone Effective But Removing Token Mixer (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2304.05659)

<p align="center">
    <img width="500" alt='fig1' src="../img/wang2023riformer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT에서 self-attention과 같은 token mixer는 서로 다른 spatial locations의 정보를 aggregate하는 방식으로 성능 향상에 큰 기여를 함
- 하지만, token mixer는 computational complexity가 매우 커서 practical한 사용이 어려움
- Local attention, sparse attention과 같이 효율적인 token mixer를 디자인하여 vision backbone을 경량화하려는 시도가 있었지만 여전히 token mixer의 복잡도가 존재함
- 최근에는 아예 token mixer를 제거하는 연구도 있었지만 큰 성능 감소가 있었음
- 따라서, 본 논문에서는 token mixer를 제거하면서도 backbone의 효과를 유지시킬 수 있는 방법에 대해 논의하고자 함
- 이를 위해, 본 논문은 우선 최근의 모델 아키텍처와 학습 패러다임을 리뷰함
> - 대부분의 연구들은 모델 아키텍처를 개선하는 데에 집중하고 기존의 supervised learning으로 모델을 최적화함
> - 반면, 본 연구에서는 매우 간소화된 모델 아키텍처를 사용하되 학습 패러다임을 주의 깊게 탐구하여 간단한 모델의 potential을 최대로 이끌어냄
- 본 논문은 token mixer 없이도 모델의 성능을 유지시키기 위해 간단하지만 효과적인 학습 전략인 knowledge distillation에 집중
> - Token mixer를 사용하는 강력한 teacher model로부터 token mixer를 사용하지 않는(token mixer free) student 모델로 지식을 전이함
> - 이외에도 token mixer free 모델을 효과적으로 학습시키기 위한 여러 학습 전략을 사용
- 이러한 논의를 바탕으로, 본 논문에서는 token mixer를 사용하지 않아 매우 효율적이면서 경쟁력있는 성능을 유지하는 vision model인 RepIdentityFormer(RIFormer)를 제안함

## 접근법
- 베이스라인 RIFormer는 MetaFormer와 전체 아키텍처 및 모델 사이즈 측면에서 동일하지만, inference 시 token mixer가 사용되지 않는다는 차이가 존재함
- 우선 베이스라인 RIFormer를 cross-entropy loss로 일반적인 supervised 학습을 시킨 후 평가를 진행함
> - Inference 시 token mixer 사용 X
> - Token mixer를 단순한 pooling 연산으로 대체한 동일 사이즈의 PoolFormer와 비교한 결과 성능이 더 낮았음
> - 이러한 결과는 supervised learning이 token mixer가 없는 상황에서는 효과적인 학습 방법이 아님을 의미함
- 따라서, 저자들은 학습 전략을 수정하고자 했으며 이는 (1) knowledge distillation의 사용, (2) teacher type influence, (3) structural re-parameterization, (4) module imitation technique, (5) load partial parameters from teachers로 요약할 수 있음
- 먼저, knowledge distillation으로 token mixer free 모델을 훈련시킴 ((1), (2))
> - Tearcher 모델은 token mixer를 사용함
> - True labels를 이용한 supervised training은 하지 않고 teacher의 soft target에 대해서만 훈련되었을 때 성능이 가장 좋았음
- 또한, powerful model을 훈련시키고 inference 시에는 간단한 모델로 전환하는 structural reparameterization 전략을 사용함
- 이때 RIFormer의 inference time 시 token mixer는 identity mapping으로 볼 수 있고, 따라서 training time의 token mixer는 다음과 같은 조건을 만족해야 함
> - Equivalent transformation을 위한 per-location 연산자여야 함
> - 추가적인 표현력을 지니기 위해 parameter가 있는 연산자여야 함
- 따라서, training 시에 RIFormer는 channel-wise scaling 및 shift를 수행하는 affine transformation 연산자를 token mixer로 사용함 ((3))
> - Affine operator와 이어지는 layer normalization layer는 수정된 weights를 갖는 layer normalization layer로 변환이 가능하므로 inference 시 indentity mapping으로 동등하게 변환할 수 있음
- 하지만, re-parameterization은 성능에 큰 영향이 없었음
> - 이것은 affine transformation이 단순한 linear 연산이며, 현재 사용하고 있는 knowledge distillation은 output에 대한 matching만을 고려하므로 추가적인 parameter의 potential이 최대로 활용되지 못하고 있음
> - 따라서, output에 대한 지식 전이가 아닌 각 layer의 모듈 각각에 대해 지식을 전이하는 방법을 추가로 적용함
- Module imitation 방법을 사용하여 teacher의 token mixer로부터 유용한 정보를 전달받을 수 있도록 함 ((4))
> - PoolFormer를 teacher로 사용함
> - Student의 각 block이 대응되는 teacher block의 input 및 output을 모방하도록 함
> - 이는 간단한 affine operator로 teacher의 token mixer 기능을 모방하는 것과 같음
- 이전의 model compression 연구에서 영감을 받아 affine transform을 제외한 다른 파라미터의 가중치를 teacher networks의 가중치로 초기화함 ((5))

## 실험결과
- Token mixer를 제거하여 매우 빠른 처리 속도를 지님에도 불구하고 성능이 기존 모델들과 매우 비슷한 수준으로 유지됨

## 의견
- /