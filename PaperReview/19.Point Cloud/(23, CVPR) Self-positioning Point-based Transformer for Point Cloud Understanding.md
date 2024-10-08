# Self-positioning Point-based Transformer for Point Cloud Understanding (CVPR, 2023)

[논문링크](https://arxiv.org/abs/2303.16450)

<p align="center">
    <img width="600" alt='fig1' src="../img/park2023self.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Point clouds는 자율 주행, 로보틱스, 증강 현실을 포함한 여러 분야에서 활발히 적용되고 있음
- Point cloud는 정돈되지 않은, irregular한 구조이므로 CNN을 point cloud에 적용하는 것은 힘듦
> - 몇몇 연구들이 point clouds를 regular structures로 변환시키고자 했으며 CNN을 적용하고자 했음
> - 하지만, CNN은 long-range dependency를 포착하는 능력이 제한적이며, point clouds의 경우 특히 real-world data일수록 global한 shape context를 이해하는 것이 중요함
- Transformer는 NLP 뿐만 아니라 CV에서도 long-range dependency issue를 해결하기 위해 사용되었음
- 하지만 Transformer의 self-attention은 매우 큰 computational complexity를 가진다는 문제가 있음
> - 이를 해결하기 위해 local attention, sparse attention과 같은 기법들이 제안됨
- Point clouds에서도 Transformer 구조가 제안되었으며, 발전된 local attention을 사용하는 연구도 있었지만 여전히 다른 vision 분야에 비해 부족함
- 따라서, 본 논문에서는 낮은 complexity로 local 및 global shape을 모두 포착할 수 있는 Transformer 구조인 Self-Positioning Point-based Transformer (SPoTr)을 제안함
- SPoTr block은 두 개의 attention modules로 구성됨
> - Local structures를 학습하기 위한 local points attention (LPA)
> - Selfpositioning points에 기반하여 global information을 포착하기 위한 self-positioning point-based attention (SPA)
> - SPA는 모든 input points에 대해 attention을 계산하는 것이 아니라 소수의 Self-Positioning points(SP points)에 대해서만 attention을 계산하여 global attention을 수행함
> - SP points는 input shape에 따라 전체적인 shape 정보를 포함할 수 있도록 적응적으로 위치함
> - SP points는 disentangled attention을 사용하여 spatial 및 semantic 유사성을 고려하면서 자신의 representation을 학습함
> - SPA는 이러한 SP points의 정보를 각 input point에 non-local하게 전달함

## 접근법
- SPoTr의 목적은 self-positioning points를 사용하는 Transformer 아키텍처를 기반으로 여러 point cloud tasks를 위한 point representations을 학습하는 것
- 먼저, SPoTr은 SPA를 사용하여 기존 self-attention의 연산량 문제를 해결하고자 함
> - SPA는 소수의 SP points에 대해서만 attention weights를 계산함
- 이때 SP points는 전체적인 shape context를 담고 있어야 하므로 SP points의 위치는 flexible해야 함
- 따라서, SP points는 local part shapes를 충분히 대표할 수 있도록 input shape에 따라 적응적으로 위치함
> - 모든 points의 features와 SP points의 latent vectors의 내적을 기반으로 input에서의 SP points 위치를 결정
- SP points가 결정되면 SPA가 수행되며, SPA는 aggregation과 distribution의 두 과정으로 구성됨
> - Aggregation step에서는 spatial/semantic proximity를 고려하여 모든 input points features를 aggregate함
> - Aggregate function은 spatial/semantic 각각의 kernel function으로 이루어짐
> - Spatial kernel function: radial basis function(RBF), Semantic kernel function: attention-based kernel
> - Spatial/semantic 정보 추출을 위한 kernel의 분리는 disentangled attention으로 해석될 수 있음
> - 이러한 두 정보의 동시 고려는 모델의 표현력을 증가시켜줌
> - Distribution step은 SP points와 input points 간의 cross-attention을 수행함 (channel-wise point attention(CWPA)를 통해 진행됨)
- CWPA는 query와 key points의 각 채널에 대해 attention weights를 계산하며, 이는 모든 채널에 걸쳐 동일한 attention weight을 생성하는 standard attention과 다름
- SPoTr block은 SPA와 함께 LPA를 사용함
- LPA는 local shape context를 학습하기 위해 정의된 local group 내에서 attention을 수행함
> - 마찬가지로 CWPA 기반으로 수행됨

## 실험결과
- 여러 point cloud tasks에서 SOTA 기록

## 의견
- /