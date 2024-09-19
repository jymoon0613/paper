# Generalizable Implicit Neural Representations via Instance Pattern Composers (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Kim_Generalizable_Implicit_Neural_Representations_via_Instance_Pattern_Composers_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/kim2023generalizable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Implicit neural representations(INR)는 neural network를 학습시켜서 data instance을 continuous function으로 표현
> - Coordinate-based MLP의 경우 2차원 좌표값을 입력으로 받아 3차원의 RGB 값을 매핑함
> - 하지만 하나의 coordinate-based MLP는 오직 하나의 instance에 대한 representations만 생성할 수 있음 (computationally infeasible)
> - 모델은 unseen instances에 대한 representations는 생성할 수 없음
- Generalizable INR은 모든 instances에 대한 common representations를 매핑하는 하나의 neural network만을 사용하는 것을 의미
> - Feature-modulation 기법은 각 instance의 latent vector에 따라 neural network의 각 layer의 activation을 조절함
> - Weight-modulation 기법은 각 instance의 latent vector에 따라 neural network의 전체 weights를 조절함
- Weight-modulation의 경우 modulation capacity가 증가하여 성능이 좋아진다는 장점이 있지만 전체 weights을 변환시키므로 training 과정이 매우 복잡해짐
- 본 논문에서는 instance pattern composer를 사용하여 일부의 weights만 변환시켜도 generalizable INRs를 매핑할 수 있는 방법을 제안함
> - 각 instance는 low-level patterns를 구성하여 표현될 수 있다고 가정

## 접근법
- Generalizable INR의 coordinate-based MLP를 구성하는 parameters를 두 부분으로 나눔
> - Data instance를 charaterize하는 instance-specific parameter $\phi$
> - 모든 instances에 공유되어 주어진 dataset의 underlying structure를 학습하는 instance-agnostice parameter $\theta$
- 이전의 weight-modulation 기법들은 먼저 instance-agnostic parameters로 학습되고, modulation function $g$를 적용하여 instance-specific parameters로 변환하는 과정을 진행함
- 본 논문에서는 대규모 weights 변환 과정 없이 instance-specific하느 contents의 간단한 patterns를 조합함으로써 complex data를 표현할 수 있다고 주장함
- 이를 위해 MLP의 weights를 다음과 같이 구분함
> - Instance-specific parameter의 역할을 하는 instance pattern composer
> - Instance-agnostic parameter의 역할을 하는 pattern composition rule
> - MLP 초기 layer에 modulate할 수 있는 instance pattern composer weight을 사용하고, 나머지 weights는 instance agnostic한 pattern composition rule로 사용함
- 먼저, 입력 좌표값을 Fourier features로 변환하고, learnable weights를 사용하여 low-level frequency patterns로 매핑함
- 이때 instance-specific한 representations를 추출하는 instance pattern composer를 사용함
> - Modulated weight matrix를 factorization함
> - Factorized matrix로 low-level frequency patterns를 매핑함
> - 즉, 본 논문에서는 하나의 weight matrix만 modulate하여 generalizable INRs를 매핑할 수 있음
- 이후의 MLP weights은 modulate 없이 모든 instances에 동일하게 적용됨
- 각 instance마다 modulate weight matrix 할당이 필요하며, weight의 적절한 학습 과정이 필요
> - Meta-learning을 통해 modulate weights의 효과적인 initialization을 학습 
> - 혹은 Transformer-based hypernetwork 사용하여 modulate weights를 end-to-end로 학습

## 실험결과
- 제안하는 방법은 하나의 coordinate-based MLP와 소수의 weight-modulation을 사용함에도 불구하고 audio reconstruction, image reconstruction, novel view synthesis 등에서 효과적인 성능을 기록

## 의견
- /