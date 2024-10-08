# Grad-CAM: Visual Explanations From Deep Networks via Gradient-Based Localization (ICCV, 2017)

[논문 링크](https://openaccess.thecvf.com/content_iccv_2017/html/Selvaraju_Grad-CAM_Visual_Explanations_ICCV_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/selvaraju2017grad.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- GAP를 사용하지 않는 모델들에 대해서도 광범위하게 적용할 수 있는 Visualization 방법 
- CAM의 일반화된 버전 

## 접근법
- CNN의 마지막 Convolution Layer가 전체 네트워크에서 가장 고수준의 의미 정보와 공간 정보를 지니고 있음 
- 따라서 Grad-CAM은 CNN의 마지막 Convolution Layer로 흐르는 Gradient 정보를 활용 

## 실험결과
- Object Localization Task 성능을 통해 Grad-CAM의 성능을 평가 

## 의견
- CAM 이미지는 원래 이미지보다 해상도가 낮음 
- 이미지 픽셀 단위의 세밀한 요소들 다루지 못함 
