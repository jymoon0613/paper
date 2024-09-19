# Restoration of Hand-Drawn Architectural Drawings Using Latent Space Mapping With Degradation Generator (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Choi_Restoration_of_Hand-Drawn_Architectural_Drawings_Using_Latent_Space_Mapping_With_CVPR_2023_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/choi2023restoration.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 오래된 목조 건축 도면 복원 task에 관해 다룸
- 오래된 도면은 보통 색과 배경이 바래고, 선이 변질되고, 이미지 일부가 흐려지므로 복원이 어려움
- 또한, noisy한 도면에 비해 clean한 도면 데이터가 매우 부족하므로 일반적인 image-pair supervision으로 학습시키기 어려움
- 본 논문에서는 VQ-VAE 기반의 복원 기법을 제안함 

## 접근법
- 먼저 VQ-VAE와 마찬가지로 clean drawings에 대해 encoder $E_c$, generator $G_c$, codebook $\mathcal{C}$를 학습시킴
> - $E_c$는 입력 이미지 $x_c$를 latent varaible $z_c$로 매핑
> - $z_c$을 $\mathcal{C}$에 대한 index $k_c$로 치환
> - Index에 따라 codebook의 codeword embedding $\hat{z}_c$를 가져와 $G_c$로 입력하고 복원을 수행 ($\hat{x}_c$)
> - Reconstruction loss와 adversarial loss, vq-loss를 통해 최적화
- 이후 noisy drawings에 대한 latent space mapping을 학습하기 위해 generator $G_c$와 codebook $\mathcal{C}$를 그대로 사용하면서 noisy encoder $E_n$을 학습시킴
> - $E_n$은 입력 이미지 $x_n$를 latent varaible $z_n$으로 매핑
> - $z_n$을 $\mathcal{C}$에 대한 index $k_n$로 치환
> - Index에 따라 codebook의 codeword embedding $\hat{z}_n$을 가져와 $G_c$로 입력하고 복원을 수행 ($\hat{x}_c$)
> - $E_c, G_c, \mathcal{C}$는 freeze 시킨 뒤 $E_n$에 대해서만 학습 진행
> - $z_c$와 $z_n$, $\hat{z}_c$와 $\hat{z}_n$ 간의 거리가 최소가 되도록 mapping loss와 refinement loss를 최적화
- $E_n$ 훈련 과정에서는 clean/noisy drawing pairs가 필요하지만 실제로는 noisy drawing에 대응하는 clean drawing이 존재하지 않는다는 문제가 있음
- 이를 위해 clean drawing에 noise를 추가한 가상의 noisy drawing을 생성하는 degradation generator $G_n$를 사용
> - $G_n$은 $z_n-\hat{z}_c$의 residual mapping errors $z_e$와 Gaussian noise를 weighted sum하여 입력으로 받음 ($z_g$)
> - $G_n$의 구조는 $G_c$와 동일하며, 단, $G_n$의 생성 과정에서 동일한 입력에 대한 $G_c$의 intermediate activations를 concat함
> - 생성된 가상의 noisy drawing $\hat{x}_g$와 $x_n$ 간의 reconstruction loss, adversarial loss로 $G_n$ 최적화
- 두 stage를 거쳐 훈련이 끝난 뒤 inference 시에는 noisy drawing이 $E_n$, $\mathcal{C}$, $G_c$를 거쳐 denosing됨

## 실험결과
- 다른 복원 기법과 비교했을 때 제안하는 방법의 복원 성능이 가장 좋았음

## 의견
- /