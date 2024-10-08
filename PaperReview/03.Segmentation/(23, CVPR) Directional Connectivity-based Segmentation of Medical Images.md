# Directional Connectivity-based Segmentation of Medical Images (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2304.00145)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2023directional.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Medical image segmentation에서 해부학적 일관성(anatomical consistency)를 유지하는 것이 중요함
- 이미지에 존재하는 해부학적 일관성은 topological properties로 표현될 수 있음 (i.e., pixel connectivity, adjacency)
- 하지만 segmentation에 사용되는 일반적인 네트워크는 pure pixel-wise classification만을 수행하며 segmentation mask를 유일한 label로 사용함
- 하지만 이러한 pixel-wise modeling을 inter-pixel relationships와 geometrical properties를 직접적으로 다루지 않으므로 suboptimal함
- 따라서, 이러한 모델들은 유사한 spatial features를 공유하는 이웃 pixels에 대한 불연속적인 예측을 수행함르로써 낮은 spatial coherence를 보일 수 있음
> - 특히 높은 noise/artifacts medical data를 다루는 경우, spatial consistency는 더 낮아질 수 있고 이는 topological issues를 유발함
- Pixel connectivity는 digital images에서 separation과 connectedness라는 두 기본적인 topological한 특성을 다루기 위해 사용되어 왔음
- 또한, 최근에는 더 발전하여 connectivity masks가 segmentation mask의 topological 확장을 위해 사용되고 있음
- Connectivity masks를 training label로 사용하는 것은 segmentation masks를 사용하는 것보다 여러 장점이 있음
> - Connectivity mask를 사용하는 것은 segmentation task를 pixel-wise classification에서 connectivity prediction 문제로 변환시켜주며 모델의 topological한 표현을 강화함
> - Label representation의 측면에서, connectivity mask는 (1) pixel 연결 간의 범주 정보를 저장하면서 pixel 간 연관성을 인식하고, (2) edge pixels를 sparse하게 표현하며, (3) 풍부한 방향 정보를 channel 수준에서 포함하고 있기 때문에 더 informative함
- 따라서, connectivity mask로 훈련된 네트워크는 latent space에서 (연결성이 반영된)categorical features와 directional features를 둘 다 학습하게 되고, 각각은 개별적인 sub-latent space를 지님
- 본 논문에서는 공유되는 latent space에서 directional subspace를 disentangle하여 directional features 추출하고, 추출된 directional features로 전체적인 데이터 표현을 강화하는 directional connectivity-based segmentation network(DconnNet)을 제안함
> - Disentangling process는 sub-path slicing-based module인 Sub-path Direction Excitation(SDE)로 수행됨
> - Directional-based feature enhancement는 coarse-to-fine 방식으로 적용되며 Interactive Feature-space Decoder(IFD)가 사용됨
> - 마지막으로 medical dataset의 data imbalance 문제를 해결하기 위해 Size Density Loss(SDL)을 제안함

## 접근법
- Connectivity-based network의 latent space에는 두 가지 종류의 features가 존재함: categorical and directional
> - 각 그룹의 features는 latent space에서 각자의 subspace를 형성하고 있음
- 이러한 두 subspace를 효과적으로 disentangling하고 direction space를 사용하는 것은 connectivity model의 전체적인 표현 학습을 향상시킬 수 있음
- Connectivity mask에서 서로 다른 채널은 서로다른 pixel connection의 direction을 표현함
- 따라서, 네트워크가 깊어질수록 모델은 directional information을 channel에 인코딩함
- 즉, channel 정보를 explore하여 directional features를 포착할 수 있음
- 이를 위해, 우선 SDE를 사용하여 latent space에서 channel-wise directional features를 추출함
> - 먼저 backbone으로 입력부터 출력까지를 계산하고, output latent feuatures로부터 element-wise directional information을 담고 있는 directional prior을 계산함
> - 이후 계산된 latent features와 directional prior을 channel-wise slicing으로 그룹화함
> - 이후 각각의 slice를 channel attention module에 입력하고, 결과를 concat하여 최종 SDE features를 얻음
> - SDE features의 channels에는 기존 latent space에서 disentangled된 direction information이 인코딩되어 있음
- 다음으로, IFD를 통해 서로 다른 layers의 directional embeddings를 추출하여 self-attention 기반의 표현 강화를 진행함
> - IFD는 top-down 방식의 space flow와 feature flow로 구성
> - Space flow를 통해 directional information을 feature map에 fuse함
> - Feature flow는 space flow의 결과를 upsample하여 하위 layer로 top-down 전달하는 역할을 함
- 학습 과정에서 SDL을 loss function에 추가하여 label size distribution 기반의 weighting 전략을 추가함 
> - Data imbalance 문제 해소

## 실험결과
- Medical image segmentation 성능 검증을 위해 Retouch, ISIC2018 데이터셋을 사용
> - SOTA 성능 기록
- Topological 성능을 확인하기 위해 CHASEDB1 데이터셋을 사용
> - SOTA 성능 기록

## 의견
- /