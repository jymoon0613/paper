# A ConvNet for the 2020s (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Liu_A_ConvNet_for_the_2020s_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/liu2022convnet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 2010대에는 deep learning, 특히 CNN의 기념비적인 발전이 있었으며 visual recognition 분야는 기존의 hand-crafted method에서 CNN 위주로 변화함
> - CNN의 sliding window 전략은 visual processing에 매우 효과적이었음
> - CNN 자체에 내재된 여러 inductive biases는 다양한 vision applications와 잘 맞아떨어짐
> - 특히, CNN의 translation invariance한 특징은 object detection과 같은 task에 있어서 매우 중요함
> - 또한, CNN은 convolution 연산의 computation을 share할 수 있으므로 매우 효율적임
> - 이러한 특징들로 인해 CNN은 recognition task를 넘어 오랫동안 여러 vision tasks의 general purpose backbone으로 자리잡았음
- 2020년 이후에는 NLP에서 활발히 사용되었던 Transformer 구조를 vision에 적용한 ViT가 등장하였음
> - ViT는 CNN과 달리 image-specific한 inductive bias를 전혀 지니고 있지 않으며 NLP Transformer를 최소한으로 변경시켜 거의 동일한 구조를 사용함
> - ViT는 scaling 측면에 특히 집중하여 더 큰 모델, 더 많은 데이터셋에 대해 CNN(i.e., ResNet)을 능가했음
- 하지만, ViT의 성능은 image classification task에서만 두드러졌으며 detection, segmentation과 같은 tasks는 여전히 기존 CNN의 sliding-window 방식에 의존하고 있었음
> - CNN의 inductive bias 없이 vanilla ViT 모델로는 이러한 generic한 vision tasks에 적용되기 어려웠음
> - 특히, ViT의 self-attention의 quadratic한 복잡도는 보통 recognition보다 큰 input resolution을 요구하는 이러한 tasks에 부적합할 수 있음
- CNN의 장점을 ViT에 결합한 hybrid approach(hierarchical Transformers)가 이러한 간극을 좁혀주었음
> - 대표적으로 Swin Transformer가 CNN의 sliding-window 방식을 Transformer 구조에 접합시켜 여러 generic한 tasks에 적용되어 SOTA 성능을 기록했음
- Swin Transformer와 같은 hybrid model의 등장과 성공은 convolution의 본질이 결코 퇴색되지 않았으며 오히려 더 중요하다는 것을 의미함
- 이러한 관점에서 Transformer의 발전 방향은 convolution의 개념을 다시 가져와 Transformer에 접목시키는 것이었음
> - 하지만 이러한 접목 방식은 많은 비용을 수반하는데, 이는 sliding window 기반의 self-attention은 계산비용이 크며, 최적화를 위해서는 정교한 디자인 과정이 필요하기 때문임
- CNN은 vision tasks에 필요한 여러 요소들을 이미 지니고 있지만 여전히 Transformer가 선택되고 있음
- 이는 Transformer가 multi-head self-attention을 통한 우수한 scalability를 바탕으로 현재 많은 vision tasks에서 CNN을 능가하고 있기 때문임
- 지난 10년간 점진적으로 발전한 CNN과 달리 ViT는 급격하게 발전하였으며, 선행 연구들은 두 구조를 시스템 수준에서 비교하였음
> - CNN과 hierarchical Transformers는 서로 유사한 inductive biases를 내재하고 있지만 training 과정과 macro/micro 수준 아키텍처 디자인에서 차이가 있음
- 본 논문에서는 CNN과 Transformer의 차이를 아키텍처 수준에서 분석하고 두 구조의 성능 차이를 유발하는 핵심 요소를 식별하고자 함
> - 이를 통해 CNN의 ViT 등장 이전과 이후의 성능 격차를 줄이고, pure한 CNN 자체의 한계도 테스트해보고자 함
- 이를 위해 기본 ResNet으로부터 시작하여 hierarchical Transformer(i.e., Swin Transformer)와 비슷한 구성으로 CNN 아키텍처를 점점 현대화(modernize)함
- 이러한 과정에서 Swin Transformer를 능가하는 순수 convolution으로만 이루어진 pure CNN인 ConvNeXt 아키텍처를 도출함

## 접근법
- ResNet-50을 베이스라인 모델로 설정
- ResNet-50을 순차적으로 개선함(modernize함)
- ImageNet-1K 분류 성능을 기준으로 평가함

### (1) Training Techniques
- 모델 아키텍처 만큼이나 training procedure도 성능 향상에 중요함
> - ViT는 새로운 모듈 혹은 아키텍처 디자인 요소를 도입하기도 했지만, 동시에 CNN과는 다른 training techniques를 도입하였음
- 따라서 DeiT, Swin Transformer와 매우 유사한 training 방법을 적용함
> - 훈련 epoch 수를 기존의 90에서 300으로 변경
> - AdamW optimizer 사용
> - Mixup, Cutmix, Random augmentation, random erasing과 같은 data augmentation 방법 적용
> - Stochastic depth, label smoothing과 같은 regularization 방법 적용
- 적용결과 ResNet-50은 기존의 76.1%에서 78.8%로 성능이 증가함
- CNN과 Transformer의 성능 격차는 이러한 training procedure의 차이에서 비롯된 것일 수 있음

### (2) Macro Design
- Swin Transformer의 전체적인 아키텍처 디자인으 분석하고 이를 CNN에 적용하고자 함
- 먼저 stage compute ratio를 변경함
> - Swin Transformer는 hierarchical 구조를 구현하기 위해 ResNet처럼 4개의 stage로 아키텍처를 구분함
> - Swin Transformer-tiny에서는 1:1:3:1 비율로 stage마다 block이 정의되어 있으며 large 모델은 1:1:9:1의 비율임
> - 따라서 기존 ResNet-50에서 사용하던 (3,4,6,3)의 stage별 residual block 개수를 (3,3,9,3)으로 변경시킴
> - 그 결과 정확도가 78.8%에서 79.4%로 높아졌고 block개수가 많아진 만큼 연산량도 증가
- 다음으로 입력 이미지의 초기 처리 단계(stem)를 변경함 
> - 원본 이미지는 불필요한 중복 정보가 많기 때문에 CNN과 ViT 모두 초기에 큰 downsampling을 적용하여 입력을 적절한 사이즈의 feature map으로 변환함
> - ResNet-50의 경우 stride=2의 7x7 convolution과 max pooling을 적용하여 입력 이미지의 resolution을 4배 downsample함
> - ViT는 큰 kernel size(i.e., 14, 16)와 non-overlapping convolution을 사용하여 이미지를 14x14 혹은 16x16의 patch로 나누며, Swin Transformer는 더 작은 단위인 4x4 크기의 patch로 나눔
> - 이러한 'patchify' 전략은 ResNet보다 공격적인 downsampling을 적용하는 것으로 볼 수 있음
> - 따라서 기존 ResNet-50의 stem cell을 stride=4의 4x4 non-overlapping convolutional layer로 변경하여 patchify layer를 구현함
> - 이는 Swin Transformer의 stem과 같은 방식으로 동작함
> - 그 결과 정확도는 79.4%에서 79.5%로 미세하게 증가하였으며 연산량은 미세하게 감소함

### (3) ResNeXt-ify
- RssNeXt의 아이디어를 적용하여 연산량-정확도 간의 더 좋은 trade-off를 이끌어내고자 함
- ResNeXt의 특징은 input channel을 32개의 group로 나누어 각자 연산을 한 후 다시 concatenate시키는 grouped convolution을 수행한다는 것
- 본 논문에서는 이러한 개념을 더욱 확장한 depthwise convolution을 적용하여 연산량을 대폭 줄임
- 넓은 의미에서 depthwise convolution은 channel 수준에서의 self-attention과 유사함
- Swin-T와 채널을 맞추기 위해 width를 64에서 96으로 증가시킴
- 결과적으로 80.5%의 정확도 달성함

### (4) Inverted Bottleneck
- Transformer의 MLP block에서는 hidden layer의 channel dimension을 4배 늘린 후 다시 원래 dimension으로 되돌림 (inverted bottleneck)
- 따라서, ConvNeXt도 마찬가지로 기존 구조에 inverted bottleneck을 적용함
> - 기존에는 {1x1 convolution으로 채널 축소(384->96) -> 축소된 채널에서 3x3 convolution 적용(96->96) -> 1x1 convolution으로 다시 채널 복구(96->384)} 순으로 진행되었던 bottleneck block을 {1x1 convolution으로 채널 증폭(96->384) -> 증폭된 채널에서 3x3 convolution 적용(384->384) -> 1x1 convolution로 다시 채널 복구(384->96)} 순으로 재설계
> - 정확도가 약 0.1%p 증가

### (5) Large Kernel Sizes
- ViT의 강점 중 하나는 non-local self-attention을 통해 각 레이어가 global receptive field를 가지도록 한 점임
- 마찬가지로 receptive field를 확장하기 위해 CNN에서도 large kernel size가 사용되기도 했지만, VGG-Net부터 시작된 3x3 kernel size를 stack하는 방법이 주를 이루었음
- 따라서, 본 논문에서는 기존 3x3의 기본 kernel size 대신 최소 7x7의 large kernel에 대해 실험함
- 먼저, depthwise convolution layer의 순서를 변경
> - 위의 inverted bottlneck 구조에서 1x1(96->384) -> 3x3(384->384) -> 1x1(384->96)의 convolution 순서를 3x3(96->96) -> 1x1(96->384) -> 1x1(384->96)으로 변경
> - Depthwise convolution이 self-attention 과정과 유사하다면 Transformer block이 self-attention 수행후 MLP block을 거치는 것처럼, bottleneck의 연산 순서를 바꾸는 것이 적합함
> - 그 결과 연산량은 감소하였지만 정확도가 조금 낮아짐(79.9%)
- 다음으로 kernel size를 증가시킴
> - 기존 depthwise convolution layer의 kernel size를 7x7로 변경
> - 정확도가 79.9%(3x3) 에서 80.6%(7x7)으로 향상

### (6) Micro Design
- Activation function, normalizeation layer와 같은 micro design에 대한 변경 적용
- 먼저 activation function을 변경함
> - NLP와 vision architecture 사이에는 activation function에 차이가 있음
> - 수많은 activation function이 개발되었지만 vision에서는 여전히 ReLU가 단순함과 효율성 측면에서 각광받음
> - Transformer도 초기에는 ReLU를 사용하였지만 최근에는 ReLU의 smoother vaiant 버전인 GELU(Gaussian Error Linear Unit)가 BERT, GPT-2, ViT 등의 최근 연구에서 많이 사용됨
> - 정확도와 연산량은 변하지 않았지만 본 연구에서는 모든 activation function을 GELU로 변경
- 또한 activation function의 수를 줄임
> - 일반적인 CNN은 conv -> normalization -> activation이 공식화 됨
> - 반면 Transformer는 훨씬 적은 수의 activation function을 사용: self-attention부분에는 activation이 없고 MLP block에서 단 한개의 activation function만 사용
> - 이러한 Transformer의 전략을 가져오기 위해 bottleneck block의 두 개의 1x1 convolution 사이에 하나의 GELU만을 사용
> - 정확도를 81.3%까지 향상
- 마찬가지로 normalization layer의 수를 줄임
> - Transformer에서는 normalization layer도 self-attention, MLP block 앞에서만 수행
> - 따라서, 한개의 Batch Normalization만을 bottleneck block의 첫번째 1x1 convolution 뒤에 적용시킴
> - 그 결과 81.4%의 성능 달성
- 다음으로 normalization layer를 변경함
> - Batch Normalization은 모델의 빠른 수렴을 촉진시키고 overfitting을 방지하는 차원에서 CNN에서 널리 쓰였음
> - 하지만 batch size에 따른 성능 변화의 편차가 심하다는 등 모델의 성능에 좋지않은 영향을 줄 수 있다는 intricacies가 있음
> - 반면 Transformer는 Layer Normalization을 통해 높은 성능을 기록함
> - 따라서 기존의 Batch Normalization을 Layer Normalization으로 변경
> - 그 결과 81.5%의 성능 달성
- 다음으로 downsampling layer를 분리함
> - ResNet은 각 stage의 첫 block에서 3x3 convolution의 stride를 2로 조절하여 내부적으로 downsampling 수행
> - 하지만 Swin Transformer의 경우 patch merging이라는 별도의 downsampling layer가 각 스테이지 사이에 존재함
> - 따라서, block 내부에서 resolution을 줄이는 것이 아니라 stride=2의 2x2 convolutional layer를 별도로 추가함
> - 또한, downsampling layer의 추가로 인한 모델 발산을 방지하기 위해 downsampling 구간에 Layer Normalization을 추가함
> - 그 결과 82.0%의 성능 달성
- 최종적으로 변경된 모델은 Swin Transformer를 능가하는 pure한 CNN이며 이를 ConvNeXt라고 칭함
- ConvNeXt는 Swin Transformer와 거의 유사한 크기, 연산량, 처리속도 등을 지니지만 shifted window attention과 같은 부가적인 모듈 없이 순수 convolution으로만 구성됨
- Swin Transformer-tiny, small, base, large(Swin-T,S,B,L)과 유사한 복잡도를 갖는 ConvNeXt-tiny, small, base, large (ConvNeXt-T,S,B,L)를 정의
> - 서로 다른 모델 사이즈는 feature channel 수와 stage별 block 개수에서 기인함
- ConvNeXt-T,B는 각각 ResNet-50,200의 modernized 버전이라고 할 수 있음
- 또한 ConvNeXt의 확장성(scalability)을 실험하기 위해 더욱 거대한 ConvNeXt-XL도 설계

## 실험결과
- ImageNet-1K에서 Swin Transformer와 직접적으로 비교하면 같은 조건하에 조금씩 ConvNeXt가 더 성능이 좋음
- 또한, RegNet/EfficientNet과 같은 강력한 CNN 베이스라인과 비교했을 때도 경쟁력있는 성능을 보여주었으며, accuracy-computation trade-off에 있어서 더 좋았음
- ViT는 inductive bias가 거의 없기 때문에 large-scale dataset(i.e., ImageNet-22K)으로 pre-train되었을 때 CNN보다 좋은 성능을 기록할 수 있음
- ConvNeXt 모델을 ImageNet-22K에서 pre-train 시킨 뒤 ImageNet-1K로 fine-tuning 했을 때 비슷한 복잡도의 Swin Transformer의 성능과 비슷하거나 더 좋았음
> - 이는 잘 디자인된 CNN의 경우 large-scale dataset에서 훈련되어도 Transformer에 뒤쳐지지 않는 성능을 낼 수 있다는 것을 의미함
- 또한, ConvNeXt-XL 모델은 ConvNeXt-L 모델보다 더 좋은 성능을 기록하였음
> -  이는 ConvNeXt가 Transformer 구조와 마찬가지로 scalable한 특성을 지닌다는 것을 의미함
- ConvNeXt는 object detection 및 segmentation tasks에서 Swin Transformer의 성능과 비슷하거나 더 좋았음
> - 이는 ConvNeXt가 general purpose backbone으로서의 역할을 할 수 있다는 것을 의미함

## 의견
- /