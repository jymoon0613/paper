# Facial Expression Recognition by De-Expression Residue Learning  (CVPR, 2018)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Yang_Facial_Expression_Recognition_CVPR_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2018facial.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- FER은 매우 어려운 과제 
- 표현하는 사람 자체의 특성이 다양함 (인종, 나이, 성별 등) 
- 표현 자체의 다양함 (사람마다 개성과 표현 스타일에 따라 같은 감정이라도 서로 다른 표정이 될 수 있음) 
- 사람은 대상의 중립적 표정 (Neutral Expression)과의 비교를 통해 표정을 인식함 
- 즉, 표정 인식은 중립적 표정 요소 (Neutral Component)와 표현적 표정 요소 (Expression Component)를 분해해서 접근할 수 있음 (Decompose) 
- 따라서, 중립적 표정과의 이미지 혹은 피처 수준 차이를 통해 표정 인식을 하려는 시도가 있었음 
- 이러한 접근 방식은 비교 대상이 되는 중립적 표정이 주어져 있는 상황에서만 가능하지만, 비교 대상이 되는 중립적 표정이 항상 주어지는 것은 아니라는 문제가 존재함 
- 따라서, 표정 이미지로부터 중립적 표정을 생성해낼 수 있는 모델에 대한 개발이 필요함 
- 본 연구에서는 CGAN 모델을 사용하여 주어진 임의의 표정 이미지로부터 중립 표정 이미지를 생성하는 모델을 학습시키고, 그 과정에서 모델 내부에 축적되는 입력 표정의 Expression Component를 추출하여 FER을 진행하고자 함 

## 접근법
### (1) Neutral Face Recognition 
- CGAN을 통해 주어진 표정 이미지로부터 중립적 표정 이미지를 생성하는 모델을 훈련 
- L1 Loss 사용 
### (2) Learning Facial Expressive Component 
- 훈련된 생성 모델의 레이어에는 Expression Component가 기록되어 있음 (De-Expression Residue)
(중립 표정 생성을 위해 Expression Component를 식별하고 제거해야 하므로) 
- 각 레이어가 뽑아내는 Expression Component를 직접 추출하고 이를 FER를 수행하기 위한 피처로 사용 
- 이를 위해, 생성 모델의 각 단계에서 피처맵을 추출하고, 추출한 피처맵에 CNN을 수행하고, FC 레이어를 거쳐 병합된 뒤 최종적인 FER을 수행하게 됨 

## 실험결과
- 데이터셋으로는 CK+, CASIA, MMI, BU-3DFE, BP4D+ 사용 
- 훈련 데이터로는 BU-4DFE, BP4D를 사용 

## 의견
- 학습을 위해 모든 표정 이미지에 대해 중립 표정 이미지가 대응하여 존재하야 함 
- GAN 모델의 단점? 
- 성능을 더욱 개선시킬 방법은? 