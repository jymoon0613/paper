# MetaFormer Is Actually What You Need for Vision (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Yu_MetaFormer_Is_Actually_What_You_Need_for_Vision_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/yu2022metaformer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transformer encoder는 tokens 간의 정보를 mixing하는 attention module(or token mixer)와 나머지 channel MLP/residual connections의 두 부분으로 구성됨
- 만약 attention module을 token mixer의 특별한 한 종류로 본다면, token mixer 부분을 여러 module로 대체할 수 있는 general한 Transformer 구조를 정의할 수 있음 (MetaFormer)
- Transformer의 성공은 attention 기반의 token mixer의 발전과 함께 이루어졌음
- 하지만, 최근에는 MetaFormer 아키텍처 하에서 보다 다양한 종류의 token mixer가 사용되기도 함
> - Attention module을 spatial MLP로 대체하고자 한 시도가 있었으며 이미지 분류 벤치마크에서 경쟁력있는 성능을 기록하였음
> - Attention module을 Fourier Transform으로 대체하는 시도도 있었으며 역시 경쟁력있는 성능을 기록
- 이러한 선행 연구로 미루어보아 본 연구진은 token mixer module이 어떤 것인지와 상관 없이 MetaFormer 자체, 즉, Transformer의 general한 아키텍처가 모델의 성능에 중요하다는 가설을 세움
- 이러한 가설을 증명하기 위해, 본 논문에서는 token mixer로 아주 간단한 non-parametric operator인 pooling을 사용해봄 (PoolFormer)
> - 놀랍게도 PoolFormer는 경쟁력있는 성능을 보여주었고, 심지어는 잘 훈련된 Transformer 및 MLP-like models의 성능을 뛰어넘기도 함
> - 이러한 결과는 본 논문의 가설을 증명함
> - 하지만, 이는 token mixer가 중요하지 않다는 의미라기 보다 어떠한 token mixer라도 사용가능하다는 의미

## 접근법
- MetaFormer는 Transformer의 다른 구성요소들은 동일하되 token mixer는 정해지지 않는 general한 아키텍처임
- Token mixer가 아닌 Transformer 구조 자체가 효과적이라는 가설을 증명하기 위해, 본 논문에서는 token mixer로 간단한 pooling operator를 사용하는 PoolFormer를 정의함
> - Pooling은 learnable parameters가 없고 hierarchical structure를 구현하기에 용이함
> - Pooling을 통해 각 토큰은 일정하게 그들의 인접한 토큰 정보를 통합하므로 이는 매우 기본적인 token mixing operation임

## 실험결과
- PoolFormer는 다른 CNN 혹은 Transformer 모델들과 비교했을 때 매우 경쟁력있는 성능을 보여줌
> - MetaFormer 구조가 성능에 중요한 요소임
- Detection task에서도 ResNet 기반의 모델보다 좋은 성능을 기록
- Segmentation task에서도 ResNet 및 ResNeXt 기반의 모델보다 좋은 성능을 기록하였고, PVT 기반 모델보다도 성능이 좋았음
- MetaFormer의 token mixer로 pooling이 아닌 더 간단한 identity mapping을 사용해도 경쟁력있는 성능을 보여줌
- 마찬가지로, token mixer로 freeze된 random matrix를 사용했을 때도 성능이 괜찮았음
> - 가설의 타당성을 뒷받침함
- MetaFormer의 각 stage 별로 token mixer를 다르게 사용하는 경우(hybrid stages) 성능이 더 좋아짐
> - i.e., 1-2 stage는 pooing, 3-4 stage는 self-attention

## 의견
- /