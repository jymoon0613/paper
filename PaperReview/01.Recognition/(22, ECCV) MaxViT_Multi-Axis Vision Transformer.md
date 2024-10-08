# MaxViT: Multi-Axis Vision Transformer (ECCV, 2022)

[논문링크](https://arxiv.org/abs/2204.01697)

<p align="center">
    <img width="600" alt='fig1' src="../img/tu2022maxvit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- ViT는 overfitting의 원인이라고 할 수 있는 inductive bias가 없기 때문에 강한 model capacity를 지니며, 좋은 성능을 내기 위해서는 엄청나게 많은 데이터 및 훈련 시간이 필요함
- 따라서, local attention과 같이 모델의 capacity를 적절히 제한하면서 scalability를 향상시키는 방안이 제시되어 왔음
- Local attention은 Transformer의 유연성과 generalizability 향상에 긍정적인 영향을 주었지만 결정적으로 ViT의 non-locality 특징을 저해하여 model capacity를 제한함으로써 특히 JFT와 같이 큰 데이터셋에서의 확장성을 저해함
- Model capacity와 generalizability 사이의 균형을 맞추기 위해 global 및 local interactions을 효과적으로 통합하고, 동시에 computational budget을 최소화하는 것은 여전히 어려운 문제임
- 따라서, 본 논문에서는 하나의 block에서 global-local spatial interactions을 모델링할 수 있는 새로운 Transformer module인 multi-axis self-attention(Max-SA)를 제안함
> - Max-SA는 서로 다른 input lengths에 적용될 수 있으며 linear complexity를 가지기 때문에 매우 유연하며 효율적임
> - Max-SA는 global receptive field를 사용하기 때문에 window/local attention에 비해 strong model capacity를 보장함
> - Max-SA는 네트워크의 모든 레이어에서 독립적인 attention module로 사용될 수 있으며 linear complexity로 인해 초기의 high-resolution stage에서도 사용가능함
- 이러한 Max-SA를 사용한 Transformer인 multi-axis vision Transformer(MaxViT)를 제안함: Max-SA와 convolutions를 연속적으로 쌓아 hierarchical하게 구성

## 접근법
- 기존의 fully dense한 attention mechanisms을 window attention 및 grid attention의 두 sparse한 형태로 decompose함
> - Non-locality 특성을 희생시키지 않으면서 기존의 quadratic complexity를 linear complexity로 축소
- 우선 입력 피처맵을 $P\times{P}$의 local block으로 나누고, 각 block 내부에서 attention을 수행 (block attention)
- 이후 global interactions를 학습하기 위해 grid attention을 수행함
> - 입력 피처맵을 $G\times{G}$의 uniform grid로 분할
> - $G\times{G}$의 grid axis에서 self-attention을 수행하여 tokens을 global하게 spatial mixing함
- MaxViT block은 위의 두 가지 타입의 attention이 순차적으로 수행됨
- Multi-axis attention 이전에 MobileNet convolution(MBConv)와 squeeze-and-excitation module을 추가함
- Stem부터 시작하여 stage가 진행될수록 spatial resolution은 반으로 줄고, channel dimension은 두배가 되는 hierarchical한 구조를 채택함

## 실험결과
- Image classification, object detection, instance segmentation, image aesthetics/quality assessment, unconditional image generation tasks에서 모델을 검증함
- 모든 tasks에서 SOTA 달성

## 의견
- /