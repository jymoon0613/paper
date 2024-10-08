# ConvNeXt V2: Co-designing and Scaling ConvNets with Masked Autoencoders (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2301.00808)

<p align="center">
    <img width="500" alt='fig1' src="../img/woo2023convnext.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Visual representation learning system의 성능은 크게 세 가지 요인에 의해 좌우됨
> - 네트워크 아키텍처, 훈련 방식, 훈련 데이터
- 이 중에서 네트워크 아키텍처를 혁신하는 방법이 주로 제안되었음
> - CNN은 여러 visual recognition tasks에 범용적으로 사용할 수 있는 feature learning이 가능하도록 했음
> - Transformer(ViT)는 데이터셋 및 모델 사이즈 증가에 따른 강한 scalability를 보여주며 최근 많이 사용되고 있음
- 최근의 ConvNeXt 아키텍처는 전통적인 CNN 모델을 최신화하여 pure한 CNN 구조로도 scalable한 architectures를 구현할 수 있다는 것을 보여줌
- 하지만, 네트워크 아키텍처의 혁신은 주로 supervised learning에 초점이 맞추어져 있음
- 최근의 visual representation learning의 흐름은 기존의 supervised learning에서 self-supervised learning으로 집중되고 있음
> - 대표적으로 MAE는 masked language modeling을 기반으로 visual representation learning을 위한 효과적인 방법으로 자리매김하고 있음
- 하지만, self-superivsed learning은 일반적으로 supervised learning을 위해 사전에 정의된 아키텍처를 사용하며, 해당 아키텍처 디자인이 fix되었다고 가정함
> - 예를 들어, MAE는 ViT 아키텍처를 사용하여 개발됨
- 네트워크 아키텍처의 디자인 요소와 self-supervised learning 프레임워크를 결합하는 것이 가능할 수 있지만, ConvNeXt 아키텍처와 MAE를 결합하는 과정에서 문제가 생길 수 있음
> - MAE는 masekd autoencoding을 수행하는 과정에서 encoder가 visible한 non-masked patches의 latent representations만을 project하도록 하며, 이러한 처리 방식은 dense한 sliding window 방식을 사용하는 standard CNN에서는 적용 불가능할 수 있음
> - 또한, 네트워크 아키텍처와 training objective 간의 관계가 적절히 고려되지 않으면 optimal한 performance를 달성하지 못할 수도 있음
> - 선행 연구로부터 CNN을 mask-based self-supervised learning으로 훈련시키는 것이 어렵고, Transformer와 CNN은 서로 다른 feature learning을 수행한다고 보고되었음
- 따라서, 본 논문에서는 네트워크 아키텍처와 masekd autoencoder를 같은 프레임워크에서 co-design하여 ConvNeXt 아키텍처를 효과적으로 훈련시키기 위한 mask-based self-supervised learning을 제안함
> - Masked autoencoder 디자인에서는 masked input을 sparse patches의 집합으로 보고 visible parts만을 처리하기 위해 sparse convolutions를 사용함
> - 또한, Transformer decoder를 하나의 ConvNeXt block으로 대체하여 전체 아키텍처를 fully convolutional하게 만듦
> - 마지막으로, feature collapse 문제를 해결하기 위해 inter-channel feature competition을 향상시키는 Global Response Normalization layer를 제안함

## 접근법
- 제안하는 방법은 매우 간단하고 fully convolutional한 방식으로 작동함
- 모델은 high masking ratio로 랜덤하게 마스킹된 input에 대해 남아있는 visual context를 사용하여 missing parts를 예측하도록 훈련됨
- ${32}\times{32}$ 크기 이미지 패치의 60%를 마스킹함
- ConvNeXt 모델을 encoder로 사용함
- Transformer 기반의 모델과는 달리 CNN은 dense convolution 연산 때문에 visible patches만을 참조하도록 하기 어려움
- 가능한 해결책으로 learnable한 masked tokens를 마스킹된 입력 이미지에 추가하는 방법이 있지만, 이는 pre-training의 효율성을 감소시키고 training과 testing 간의 불일치를 유발할 수 있음
- 따라서, 본 연구에서는 sparse convolution을 사용하여 masekd image를 sparse pathces의 집합으로 처리함
> - Pre-training 시 네트워크의 convolution layer를 sparse convolution으로 하여 visible data points만 처리하도록 함
> - Sparse convolution layers는 추가적인 조작 없이 fine-tuning stage에서 standard convolution으로 전환됨
> - Sparse convolution 말고 dense convolution operation을 수행한 뒤 binary masking을 적용하는 방법도 있지만, sparse convolution이 더 효율적임
- Decoder로는 ConvNeXt block 하나를 사용함
- Reconstruction target은 MAE와 마찬가지로 normalize된 원본 이미지 패치이며, 원본 이미지와 복원된 이미지 간의 MSE loss가 masked patches에 대해서만 계산됨
- 위의 요소들을 결합하여 fully convolutional masked autoencoder(FCMAE)를 제안하며, ImageNet-1K를 사용한 end-to-end fine-tuning 성능으로 FCMAE의 각 요소들에 대한 ablation study를 진행함
- 먼저, pre-training 시 masked input 전체에 dense convolution을 적용하는 것보다 sparse convolution을 사용하여 masked region은 고려하지 않는 것이 성능이 더 좋았음
- FCMAE는 ConvNeXt 논문에서 보고된 supervised 모델 성능에 비해서는 약간 성능이 낮았음
> - 이는 masked image modeling으로 pre-trained된 모델이 supervised counterparts를 크게 능가하는 Transformer 기반의 연구들과는 상반된 결과임
> - 저자들은 이러한 결과가 ConvNeXt encoder를 사용함으로 인해 발생하는 문제와 관련있다고 판단함
- FCMAE로 훈련된 ConvNeXt 모델의 feature map channels 일부를 시각화해본 결과 feature collapse 현상이 발견됨
> - 많은 수의 feature channels가 거의 activate되지 않거나 과하게 activate됨
- Feature collapse 현상을 조사하기 위해 feature cosine distance analysis를 수행함
> - Feature map channels 간의 cosine distance를 계산함
> - High distance value는 보다 diverse한 features를 의미하며, low distance value는 feature redundancy를 의미함
> - FCMAE로 훈련된 ConvNeXt 모델의 feature cosine distance value는 supervised ConvNeXt 모델 및 MAE로 훈련된 ViT보다 작았음
- Feature map의 diversity를 증가시키는 방법으로 response normalization 기법을 사용할 수 있음
- 본 논문에서는 새로운 response normalization layer인 global response normalization(GRN)을 제안하여 feature channels의 contrast와 selectivity를 증신시키고자 함
- GRN은 (i) global feature aggregation, (ii) feature normalization, (iii) feature calibration의 세 단계로 구성됨
> - 먼저 입력 feature map $X\in{R^{H\times{W}\times{C}}}$을 $gx\in{R^C}$로 aggregate함
> - 이때 각 channel마다 L2-norm을 계산하여 aggregate함 ($gx_i=||X_i||$)
> - 다음으로 response normalization function $\mathcal{N}$을 적용함
> - $gx$의 각 element에 대해 일반적인 normalization을 수행함 ($\mathcal{N}(gx_i)=gx_i/\sum{gx_j}$)
> - 이러한 normalization 과정은 다른 channels와 비교했을 때 해당 channel의 상대적인 중요도를 계산하는 것이라 볼 수 있음
> - 마지막으로 입력 feature map을 계산된 feature normalization scores로 보정함
> - 추가로 learnable parameters인 $\gamma$와 $\beta$를 추가하여 optimization을 용이하게 했으며 GRN의 input과 output을 residual connection으로 연결함 
- GRN 대신 다른 feature normalization methods(i.e., LRN, BN, LN)를 사용하는 경우 성능이 좋지 못했으며 feature gating methods(i.e., SE, CBAM)를 사용하는 경우 성능은 비슷했지만 GRN이 더 간단하고 효율적이었음
- GRN layer를 추가하고 기존의 LayerScale을 제거하여 ConvNeXt V2를 아키텍처를 디자인함
- ConvNeXt V2 아키텍처를 사용하여 FCMAE로 pre-train 시켰을 때 기존의 feature collapse issue를 효과적으로 해결할 수 있었음

## 실험결과
- ImageNet-1K에서 ConvNeXt V2-Huge 모델은 SOTA를 달성함
- 모델 아키텍처 수정 없이 ConvNeXt 구조에 FCMAE를 바로 적용했을 때의 성능은 제한적이었으며, GRN이 추가된 ConvNeXt V2 모델의 supervised 성능은 유의미하지 않았음
> - 이러한 결과는 네트워크 아키텍처와 self-supervised learning 프레임워크의 co-design이 중요하다는 것을 보여줌
- FCMAE로 훈련된 ConvNeXt V2 모델은 모든 모델 사이즈에서 supervised ConvNeXt 모델 성능을 능가함으로써 강한 model scaling 능력을 보여줌
- 다른 masekd image modeling methods(i.e., Swin w/ SimMIM, ViT w/ MAE)와 비교했을 때도 유사하거나 더 좋은 성능을 보여주었음
- Detection 및 segmentation에서의 transfer learning 성능도 매우 좋았음

## 의견
- / 