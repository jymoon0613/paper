# Drag Your GAN: Interactive Point-based Manipulation on the Generative Image Manifold (SIGGRAPH, 2023)

[논문링크](https://arxiv.org/abs/2305.10973)

<p align="center">
    <img width="800" alt='fig1' src="../img/pan2023drag.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- GAN은 이미지 생성 분야에서 엄청난 성공을 거두었음
- GAN과 같은 learning-based image synthesis methods의 핵심적인 기능적 요구사항은 생성된 visual content에 대한 controllability를 획득하는 것임
> - 자동차 디자이너가 자신의 디자인을 여러 shape으로 interactive하게 바꿔보고자 하는 경우
- Controllable한 이미지 생성은 이상적으로 다음과 같은 특성을 지니고 있어야 함
> - Flexibility: 생성된 objects의 position, pose, shape, expression, and layout 등 여러 spatial attrubutes를 control할 수 있어야 함
> - Precision: spatial attrubutes를 매우 정확하게 control할 수 있어야 함
> - Generality: 광범위한 object categories에 대해서 모두 잘 적용될 수 있어야 함
- 기존의 방법들은 annotated training data를 사용하여 GAN의 controllability를 획득하고자 했음
> - 새로운 object categories에 일반화되지 못하며 매우 제한적인 spatial attributes만 control할 수 있었음
- 본 논문에서는 이미지의 임의의 점을 "drag"하는 방식으로 GAN을 control하는 방법을 제안함 (DragGAN)
> - 사용자는 이미지에서 임의의 두 점(handle points, target points)을 선택하며 handle points는 대응되는 target points로 도달함
> - 이러한 point-based manipulation은 여러 spatial attributes를 object categories와 무관하게 control할 수 있도록 해줌
- 이를 위해 DragGAN은 두 가지 sub-problems를 다루어야 함
> - (i) handle points가 targets points로 이동하도록 적절히 감독하는 것(motion supervision)과 (ii) handle points를 추적하여 각 editing step에서 위치를 식별할 수 있도록 하는 것(precise point tracking)
- 본 저자들은 GAN의 feature space이 motion supervision과 precise point tracking을 모두 가능하게 할 만큼 충분히 discriminative하다고 주장함
> - motion supervision은 shifted feature patch loss로 구현할 수 있으며, precise point tracking은 feature space에서의 nearest neighbor search로 구현할 수 있음
- DragGAN은 사용자가 이미지의 특정 영역에 대해서만 editing을 할 수 있도록 하는 기능도 구현할 수 있음
- 또한, DragGAN은 부가적인 네트워크를 사용하지 않으므로 보다 빠른 처리(editing)가 가능함

## 접근법
### (1) Overview
- 기본적으로 StyleGAN2 아키텍처를 사용함
- GAN으로 생성된 임의의 이미지에 대해 사용자는 handle points와 그에 대응되는 target points를 입력함
> - Handle points에 위치한 object의 semantic positions를 target points로 이동시켜야 함
> - 또한, 사용자는 이미지의 일부 region만 movable하도록 binary mask를 설정할 수도 있음
- 사용자의 입력이 들어오면 optimization 방식으로 image manipulation을 수행
- Optimization step은 (i) motion supervision과 (ii) point tracking으로 구성됨
> - Motion supervision에서는 latent code $\mathbf{w}$를 최적화하는 방식으로 handle points를 target points로 이동시킴
> - 한 번의 optimization step 이후, 새로운 latent code $\mathbf{w}^\prime$과 새로운 이미지(object가 일부 이동)를 얻을 수 있음
> - Motion supervision은 handle point를 대응되는 target point로 이동시키지만 얼만큼 이동하는지에 대해서는 불분명하며, 따라서 objects 및 parts에 따라 다를 수 있음
> - 이에 object에서의 points를 추적하기 위해 handle points를 업데이트해야 함
> - 만약 handle points가 잘 tracking되지 못하면 다음 motion supervision에서 잘못된 방향으로 업데이트가 이루어질 수 있음
> - Tracking 이후, 새로운 handle points와 latent codes에 대해 optimization step을 반복적으로 수행
> - 반복은 handle points가 target points에 도달할 때까지 수행되며 보통 30-200 iterations가 소요됨

### (2) Motion Supervision
- Generator의 intermediate features는 매우 discriminative하기 때문에 간단한 loss를 추가하는 것만으로도 motion을 supervise할 수 있음
> - StyleGAN2의 6번째 block의 output feature maps $\mathbf{F}$를 사용함
- 먼저, Bilinear interpolation을 통해 $\mathbf{F}$를 final output image와 동일한 크기로 resize해줌
- Handle point $\mathbf{p}_i$를 target point $\mathbf{t}_i$로 옮기기 위해 motion supervision loss를 정의함
> - $\mathbf{p}_i$를 기준으로 반경 $r_1$ 내에 있는 pixels($\mathbf{q}_i$)를 $\mathbf{t}_i$로 조금씩 이동시킴
> - $\mathbf{F}(\mathbf{q}_i)$와 $\mathbf{F}(\mathbf{q}_i + \mathbf{d}_i)$간의 L1 loss를 계산함
> - 이때 $\mathbf{d}_i$는 $\mathbf{p}_i$에서 $\mathbf{t}_i$로 향하는 normalized vector임
> - Binary mask가 주어지는 경우 unmasked region에 대해서는 업데이트되지 않도록 함
> - Motion supervision loss는 모든 handle points 및 대응되는 target points에 대해 summed up 됨
- 각 motion supervision step에서 latent code $\mathbf{w}$는 motion supervision loss를 통해 한 step씩 업데이트됨

### (3) Point Tracking
- Motion supervision 이후 새로운 latent code $\mathbf{w}^\prime$, 새로운 feature maps $\mathbf{F}^\prime$, 새로운 이미지 $\mathbf{I}^\prime$을 얻을 수 있음
- 이때 object의 변화에 맞게 handle point $\mathbf{p}_i$를 적절히 업데이트 시켜야 함
- Feature patch를 사용한 nearest neighbor search로 구현할 수 있음
> - $\mathbf{F}^\prime$에서 일정 반경 $r_2$ 내에 있는 pixels($\mathbf{q}_i$)에 대해 initial handle points feature $\mathbf{f}_i=\mathbf{F}_0(\mathbf{p}_i)$와 가장 가까운 point $\mathbf{q}_i$를 최적의 handle point로 결정함
> - 이때 거리는 L1 distance를 사용하며 $\mathbf{F}_0$는 원본 이미지에 대한 feature maps임
- 모든 handle points에 대해 동일한 tracking 방법이 적용

## 실험결과
- 여러 데이터셋에서 pre-trained된 StyleGAN2를 사용함
- DragGAN은 UserControllableLT에 비해 handle points를 target points로 잘 이동시켰으며 다양하고 자연스러운 manipulation 결과를 보여주었음
> - UserControllableLT의 경우 사람의 옷이 변하거나 차종이 변하는 등의 예상치 못한 변동이 발생하기도 했음
- 여러 tracking methods와 비교했을 때 제안하는 방법은 handle point를 더 정확히 tracking했으며, 따라서 editing 결과도 더 정교했음
- Real image를 manipulate하는 경우 GAN inversion을 사용하여 real image를 StyleGAN의 latent space로 embedding하는 방법을 사용함
> - 이를 통해 이전과 마찬가지로 자유로운 manipulation이 가능함

## 의견
- / 