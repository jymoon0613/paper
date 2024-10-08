# Look-Into-Object: Self-Supervised Structure Modeling for Object Recognition (CVPR, 2020)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Zhou_Look-Into-Object_Self-Supervised_Structure_Modeling_for_Object_Recognition_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhou2020look.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이미지 분류에서 중요한 것은 이미지 내에 존재하는 Object의 구조를 정확히 식별하는 것 
- 인식된 바퀴가 앞바퀴인지 뒷바퀴인지 등의 어려운 문제 해결을 위해서는 특히 Object의 전체적인 구조를 명확히 이해하고 있어야 함 (혹은 Bounding Box 같은 추가적인 Annotation이 필요함) 
- 따라서, 본 연구에서는 추가적인 Annotation이나 Extra Inference Time 없이 지역에서 문맥 정보를 자동으로 모델링하여 Object의 전체적 구조를 모델링하는 Look-into-Objects (LIO)를 제안함 
- Object의 전체적인 구조 파악은 대략적으로 물체의 위치를 식별하는 단계와, 식별된 Object로부터 세부적인 구조 정보를 식별하는 단계로 나누어 볼 수 있고, 제안하는 방법도 이러한 순서를 따름 

## 접근법
### (1) Overview
- 세 가지 모듈로 구성됨: Classification Module (CM, 백본 네트워크), Object-Extent Learning Module (OEL, 주어진 이미지에서 주요 오브젝트를 Localization하는 모듈), Spatial Context Learning Module (SCL, CM의 특징 셀 간의 상호 작용을 통해 영역 간의 연결을 강화하는 Self-supervised 모듈) 
- OEL과 SCL은 Inference 시에는 사용되지 않음 (오직 CM만 사용) 
### (2) Object-Extent Learning (OEL) 
- 주어진 이미지에서 메인 오브젝트를 Localization하는 역할을 담당 
- 같은 category에 속하는 이미지는 일종의 공통성을 공유함 
- 이러한 공통성은 주어진 클래스의 메인 오브젝트를 Localization하는 데 효과적 
- 이미지의 모든 픽셀 위치에서 같은 클래스의 이미지들과 Pixel-level Similarity를 구함 
- 이를 통해 Mask를 생성함 
- 백본의 출력에 1x1 Convolution으로 생성한 Pseudo-mask와 Correlation 기반 마스크의 MSE 손실 함수를 계산하여 훈련 
### (3) Spatial Context Learning (SCL) 
- Localization된 메인 오브젝트의 구조를 Self-supervised 방식으로 학습하는 모듈 
- 백본의 출력에 1x1 Convolution과 ReLU를 적용하여 피처 추출 
- 서로 다른 영역 간의 공간적 연결을 측정하기 위해 극좌표를 적용 

## 실험결과
- 일반적 이미지 분류, FGVC, Object Detection등 여러 태스크에서 유의미한 성능 기록 

## 의견
- OEL 방식 사용? 