# Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks (ICCV, 2017)

[논문 링크](https://openaccess.thecvf.com/content_iccv_2017/html/Zhu_Unpaired_Image-To-Image_Translation_ICCV_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhu2017unpaired.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 주어진 이미지를 목표로 하느 출력 이미지로 변환하는 image-to-image translation 문제를 다룸
- 이러한 image-to-image translation은 모델이 입력과 출력 쌍으로 구성된 데이터셋(paired data {x,y})으로 훈련함으로써 달성할 수 있지만, 이러한 paired data를 수집하는 것은 어려움
- 따라서, 본 논문에서는 paired data를 사용하지 않고도(unpaired data) 두 도메인 (X, Y) 간에 이미지를 변환할 수 있는 모델을 제안함
- 이를 위해 두 도메인은 어느정도 관련이 있다고 가정하며, 이는 확실하게 paired된 훈련 데이터가 없더라도 생성 모델이 두 도메인 간의 종합적인 특성을 비교할 수 있다는 것을 의미함
- 하지만 확실한 paired data가 주어지지 않으므로 generator는 도메인 X로부터 도메인 Y로 향하는 매핑을 학습하되, X 도메인의 한 입력 이미지 x가 Y 도메인의 이미지 y와 의미있게 매핑되는 것을 보장하지 못함
- 즉, generator는 단지 생성 이미지가 Y 도메인에 속한 이미지인 것처럼만 생성하면 되므로 구체적인 매핑을 고려하지 않고, 이러한 특성은 모델의 최적화 과정을 어렵게 할뿐만 아니라 모든 입력 이미지가 하나의 출력 이미지로만 매핑되는 mode collapse를 유발할 수 있음
- 본 논문에서는 이러한 문제를 극복하기 위해 translation 과정이 cycle consistent 해야한다고 주장함: 도메인 X를 도메인 Y로 변환하는 generator(G) 외에도 도메인 Y를 도메인 X로 변환하는 역방향 generator(F)도 학습시킴
- 이를 통해 F(G(x))=y, G(F(y))=x 가 되도록 하는 cycle consistency loss를 정의하고 이를 adversarial loss와 결합함으로써 unpaired image-to-image translation model을 효과적으로 학습시킬 수 있음 (CycleGAN)

## 접근법
- CycleGAN은 X 도메인 이미지로부터 Y 도메인 이미지를 생성하는 generator(G)와 Y 도메인 이미지로부터 X 도메인 이미지를 생성하는 generator(F)의 두 가지 generator를 사용함
- Discriminator도 생성된 X 도메인 이미지와 실제 X 도메인 이미지를 구분하는 discriminator D_x와 생성된 Y 도메인 이미지와 실제 Y 도메인 이미지를 구분하는 discriminator D_y의 두 가지를 사용함
- 우선 (G, D_y), (F, D_x)를 사용한 기본적인 adversarial loss를 정의함
- 또한, L1 loss를 기반으로 F(G(x))=y, G(F(y))=x 가 되도록 하는 cycle consistency loss를 정의함
- Total loss = adversarial loss + cycle consistency loss
- 이때, discriminator 구조로는 PatchGAN 사용
- 또한, 기존의 adversarial loss에서 negative log-likelihood loss를 least square loss로 변경
- 생성된 이미지 중 가장 최근의 50개를 따로 저장해 discriminator가 이를 한꺼번에 분류하도록 함으로써 모델 진동(oscillation) 최소화

## 실험결과
- 엄청나게 향상된 생성 결과를 보여주지는 못했지만 완전한 paired 데이터로 훈련한 Pix2Pix 모델과 비슷한 생성 퀄리티를 보여줌 
- 모델 성능을 (1) human study를 통해 생성된 이미지가 사람에게도 perceptual하게 realistic한지 검증하였으며, (2) 사진으로부터 label을 만들어내는(photo->label) 태스크와, (3) label로부터 사진을 만들어내는(label->photo) 태스크에 대해서 평가함
- 3가지 태스크 모두에서 기존의 image-to-image translation 베이스라인 모델들의 성능을 능가함 

## 의견
- /