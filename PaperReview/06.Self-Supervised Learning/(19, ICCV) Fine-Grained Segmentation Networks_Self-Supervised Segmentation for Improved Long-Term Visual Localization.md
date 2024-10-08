# Fine-Grained Segmentation Networks: Self-Supervised Segmentation for Improved Long-Term Visual Localization (ICCV, 2019)

[논문 링크](https://openaccess.thecvf.com/content_ICCV_2019/html/Larsson_Fine-Grained_Segmentation_Networks_Self-Supervised_Segmentation_for_Improved_Long-Term_Visual_Localization_ICCV_2019_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/larsson2019fine.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Long-term Visual Localization은 시간이 지남에 따라 모양이 변하는 장면에서, 주어진 쿼리 이미지의 카메라 포즈를 추정하는 문제 
- 이러한 문제에서, 변화에 대한 Robustness를 얻기 위해 각 장면 부분의 의미론적 의미가 계절적 및 기타 변화에 의해 영향을 받지 않아야 하기 때문에 종종 Semantic Segmentation을 사용함 
- 하지만 Semantic Segmentation는 소수의 클래스에 대해서만 사용 가능하므로 Discriminative Power가 낮음 
- 즉, Segment할 수 있는 클래스 자체가 적음 
- 따라서, 본 연구에서는 Image Segmentation을 위한 세분화된 클래스를 정의하기 위해 Self-supervised 기반의 접근 방식을 제안함 (Clustering 기반) 
- 즉, 인간이 정의한 의미론적 작은 클래스 세트를 사용하는 대시, 더욱 세분화된 큰 클래스 세트를 사용하여 Localization 성능을 개선시킴 
- 세분화된 클래스는 Self-supervised한 방식으로 학습됨 

## 접근법
- 기본 구조는 일반적인 CNN과 똑같음 
- 모델은 입력 이미지에 대해 Dense한 Segmentation Map을 생성함 
- 하지만 Label은 Manual Annotation이 아닌 Self-supervised한 방식으로 생성되었음 
- 훈련의 일정 간격마다 이미지로부터 Feature가 추출되고, 클러스터링 됨 
- 각 픽셀마다의 클러스터 레이블은 Segmentation Label이 됨 
- 이러한 방식으로 클래스의 개수를 계속 변경하고, 새로운 클래스를 할당할 수 있음 

## 실험결과
- Image Segmentation 데이터셋에서 훈련함 

## 의견
- 활용할 방법은? 
- 클러스터링 기반의 Image Segmentation 활용? 
