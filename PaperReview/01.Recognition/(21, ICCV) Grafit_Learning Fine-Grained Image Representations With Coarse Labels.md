# Grafit: Learning Fine-Grained Image Representations With Coarse Labels (ICCV, 2021)

[논문 링크](https://openaccess.thecvf.com/content/ICCV2021/html/Touvron_Grafit_Learning_Fine-Grained_Image_Representations_With_Coarse_Labels_ICCV_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/touvron2021grafit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-grained한 경우 레이블이 지정된 데이터를 수집하기 어려움 
- 따라서, 약한 수준의 Label만 사용하여 이미지 분류와 Image Retrieval 성능을 높이는 방법을 제안함

## 접근법
- 기본적으로 BYOL 구조를 사용하여 표현 학습 수행 
- BYOL Loss와 KNN Loss를 결합 
- Inference에는 Online Network만 사용 

## 실험결과
- CUB-200-2011, Stanford Cars iNaturalist와 같은 Fine-grained 이미지 분류 데이터셋에서 평가함 

## 의견
- 일반적인 이미지 분류 문제는 아님 