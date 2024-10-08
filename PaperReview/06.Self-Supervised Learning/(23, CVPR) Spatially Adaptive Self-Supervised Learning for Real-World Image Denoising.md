 # Spatially Adaptive Self-Supervised Learning for Real-World Image Denoising (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Li_Spatially_Adaptive_Self-Supervised_Learning_for_Real-World_Image_Denoising_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/li2023spatially.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 가상의 noise를 통해 학습한 image denoising 모델은 real-world scenarios에서 성능이 낮다는 문제가 있음
- Real-world clean-noisy pairs를 수집하여 학습하는 경우, 데이터 수집 과정이 매우 어렵다는 문제가 있음
> - 엄격한 환경 통제 필요, 복잡한 photographing과 후처리, 노이즈는 카메라와 조도에 따라 매우 다양하게 나타남
- 이에 noisy images만을 사용하는 self-supervised learning이 image denoising에 사용되었음 (self-supervised image denoising, SSID)
- 하지만 SSID의 경우 대부분 real-world noise의 특징과는 동떨어져 있는 학습 방식이라는 문제가 있음
- 본 논문에서는 noisy image의 flat regions와 textured regions 각각의 특성을 고려하여 real-world noisy image에 adaptive한 SSID 기법을 제안함
> - Flat regions와 textured regions 각각에 대한 supervised signals를 추출
> - Flat regions: blind-neighborhood network(BNN)
> - Textured regions: locally aware network(LAN)
> - 학습 이후 BNN과 LAN은 모두 detach 되고 denoising network만 inference에 사용됨

## 접근법
- Flat regions에 대한 denoising은 기존의 blind-spot network(BSN)를 확장한 BNN을 이용
> - BSN은 pixel-shuffle downsampling(PD) 이후, downsampled sub-image에서 spatially independent한 noise를 제거함
> - 하지만 PD는 high-frequency information 붕괴를 유발하고, BNN은 noise-correlated neighbor pixels와 함께 non-neighboring pixels도 제거하여 denoising 성능이 떨어짐
> - BNN은 PD 대신 BSN의 bline-spot을 모든 noise-correlated pixels를 cover할 수 있을만큼 확장함
> - BSN은 BNN과 달리 uncorrelated noisy pixels를 모두 활용할 수 있으며 기존 정보를 가능한 잘 보존함
- BNN output의 표준편차를 계산하여 주어진 영역이 flat regions인지 textured regions인지 판단
> - Textured regions인 경우 BNN만으로는 완벽한 denoising이 어려움
- Textured regions에 대해서는 LAN이 사용됨
> - LAN은 local receptive를 사용하여 neighboring pixels로부터 texture details를 복원함
- U-Net을 denoising network로 설정하고 학습된 BNN과 LAN이 생성하는 supervised signals를 사용하여 학습 수행

## 실험결과
- SIDD와 DND 데이터셋 사용
- 제안하는 방법은 모든 unpaired 및 self-supervised learning보다 성능이 좋았음

## 의견
- /