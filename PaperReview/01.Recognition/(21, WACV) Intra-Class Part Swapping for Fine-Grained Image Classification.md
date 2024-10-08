# Intra-Class Part Swapping for Fine-Grained Image Classification (WACV, 2021)

[논문 링크](https://openaccess.thecvf.com/content/WACV2021/html/Zhang_Intra-Class_Part_Swapping_for_Fine-Grained_Image_Classification_WACV_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2021intra.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝은 여러 CV 태스크에서 좋은 성능을 거두었지만, 이를 위해서는 오버피팅과 낮은 일반화를 유발하는 상당히 많은 모델 파라미터를 학습시켜야 한다는 한계가 있음
- 이러한 문제 해결을 위해 data augmentation 및 regularization 방법이 제안되었으며 최근의 mixing 기반의 augmentation 방법은 모델의 일반화를 향상시키기 위한 새로운 방법으로 떠오르고 있음
- Mixing 기반의 방법은 일반적으로 랜덤한 이미지 들의 contents를 blending하고 labels도 같은 비율로 noisy하게 만드는 방식으로 모델을 regularize함
- 하지만 이러한 방법은 fine-grained object가 공유하고 있는 공통적인 part structure를 저해한다는 점에서 Fine-Grained Image Classification (FGIC)에는 부적합함
- 또한, 이미지의 픽셀 수와 fine-grained한 정보가 항상 비례하는 것은 아니므로 이미지 contents의 혼합 비율에 따라 label을 혼합하는 것은 바람직하지 않은 label noisy를 유발함
- 따라서, 본 연구에서는 mixing될 이미지와 레이블 쌍 모두에 사전 제한을 가하는 Intra-class Part Swapping (InPS) 방법을 제안함
- InPS는 같은 클래스의 이미지들 중 혼합을 위한 입력 이미지 쌍을 랜덤하게 선택하고, attention pool을 통해 두 이미지의 potential part regions 간의 content swapping을 진행함
- 이러한 InPS 방법은 다른 mixing 기반의 augmentation 방법과 달리 fine-grained 이미지의 object structure를 과하게 손상시키지 않으며 label noise도 방지함

## 접근법
- 우선 입력 이미지에 대해 supervised하게 학습된 네트워크에서 CAM을 적용하여 attention map을 추출
- 추출된 attention map에 threshold를 적용하여 binary mask 생성
- 이러한 과정은 네트워크의 여러 레이어에서 각각 추출된 attention map에 모두 적용되며 하나로 통합됨
- 같은 클래스에 속하는 두 이미지가 랜덤하게 추출되며 위 과정을 거쳐 각각 binary mask가 생성
- 섞일 패치를 제공하는 internal attended image는 binary masking을 통해 남아 있는 part regions를 사각형 형태로 추출
- 섞이는 바탕이 되는 external attended image에 추출된 internal attended zone이 추가됨

## 실험결과
- CUB-200-2011, Stanford Cars, Aircraft 데이터셋에서 검증함
- Weakly-supervised object localization에서 다른 augmentation 방법보다 좋은 성능을 거둠
- FGIC에서의 다른 벤치마크 모델들의 성능을 개선

## 의견
- Data augmentation 관련 논문 처음 읽어봄
- 훈련 방법이나 간단한 모듈 설계만 된다면 ResNet-50 같은 모델로도 좋은 성능 낼 수 있음