# A Style-Based Generator Architecture for Generative Adversarial Networks (CVPR, 2019)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2019/html/Karras_A_Style-Based_Generator_Architecture_for_Generative_Adversarial_Networks_CVPR_2019_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/karras2019style.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 style transfer 문헌에서 영감을 받아 GAN의 generator를 이미지 생성 과정을 건트롤 할 수 있도록 re-design함
- 본 논문에서 제안하는 generator는 입력으로부터 각 convolution layer를 거치면서 style을 조정함

## 접근법
- Latent vector를 바로 입력으로 사용하는 이전의 generator와 달리, latent vector의 non-linear mapping을 먼저 수행
> - 8-layer MLP 사용
> - 입력 latent vector와 출력 latent vector의 차원은 동일 (512)
- 출력 latent vector는 AdaIN을 거쳐 각 layer로 입력됨
- AdaIN은 latent vector을 normalization하면서 style에 영향을 주도록 함
> - Affine transform으로 dimension을 맞춰줌
> - Latent vector를 normalize하고 style coefficient 및 style bias를 곱하고 더함 -> style을 입히는 과정
- 또한, 가우시안 노이즈를 각 레이어에서 더해줌
> - 이러한 per-pixel noise는 머릿결, 주근깨, 모공과 같이 recognition에 큰 영향을 주면서 랜덤하게 생성되는 부분을 표현할 수 있도록 함
- Progressive GAN을 기본 모델로 사용함
- Style에 대한 localize를 위해 mixing regulariztion을 수행함
> - 두 개의 latent vectors를 사용하여 특정 레이어 이후로는 사용되는 latent vector 변경 (위치는 랜덤)
> - 이는 인접한 style들끼리의 상관성을 제거하여 각 layer와 block이 특정한 style을 표현하도록 강제함
- 낮은 resolution을 처리하는 초반 layer에서는 전체적인 style을 처리하고 높은 resolution을 처리하는 후반 layer에서는 세부적인 style을 처리함
- Style은 pose와 identity와 같은 global한 영향을 포괄함 (per-pixel noise는 stochastic variation만 담당)
- Perceptual path length, linear separability를 통해 non-linear mapping이 수행된 latent vector가 더 disentanglement하다는 것을 증명
> - 기존의 latent vector는 각 요소가 얽혀있어서 (entanglement) visual attribute를 조절하기 힘듦

## 실험결과
- 이미지 생성 과정에서 다양한 style 적용 가능

## 의견
- /