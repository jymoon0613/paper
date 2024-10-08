# FERAtt: Facial Expression Recognition With Attention Net (CVPR, 2019)

[논문 링크](https://openaccess.thecvf.com/content_CVPRW_2019/html/MBCCV/Fernandez_FERAtt_Facial_Expression_Recognition_With_Attention_Net_CVPRW_2019_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/marrero2019feratt.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 기존의 FER 방법론에서는 이미지 내의 얼굴을 Crop 하지 않고 모든 이미지에 대해 처리함 
- 하지만 이는 분류 성능에 부정적인 영향을 미치며, 자칫 모델은 얼굴 이외의 배경 요소에서 분류 근거를 학습할 수 있음 
- 다른 방법들은 Crop을 사용하지 않지만, Heuristic한 방법을 사용하여 이미지에서 얼굴 영역을 넘어선 물체를 고려하지 않도록 제한하기도 함 
- 따라서 본 연구에서는 Attention Mechanism을 사용하여 이미지에서 얼굴 영역에 인식을 집중하여 주변 요소의 방해를 억제하고 분류 성능을 끌어올리는 방법을 제안함 

## 접근법
### (1) Network Architecture 
- 모델은 Attention Map 추출을 위한 Attention Module과 기본적인 Feature 추출을 위한 Feature Extraction Module, 적절한 Attention Image 추정을 위한 Reconstruction Module, 얼굴 표정 이미지의 표현과 분류를 담당하는 Representation Module로 구성됨 
### (2) Attention Module 
- Encoder-Decoder 구조로 되어 있음 
### (3) Feature Extraction Module 
- 4개의 Residual Block으로 구성되어 있음 
### (4) Reconstruction Module 
- Attention Map을 조정하는 역할 
### (5) Representation and Classification Module 
- FC Layers로 구성 
- 분류를 담당 
### (6) Loss Function 
- Attention, Representation, Classification Loss를 조합하여 사용
- 얼굴 영역만 마스킹된 이미지와 Attention Map을 비교하여 Attention Loss 계산 
- True Label과 Predicted Label을 비교하여 Classification Loss 계산 
- Representation Vector와 True Label을 비교하여 Representation Loss 계산 
### (7) Synthetic Image Generator 
- Labeled Image를 더욱 많이 사용하기 위해 임의로 Data를 생성함 (이미지 합성) 

## 실험결과
- CK+와 BU-3DFE 데이터셋에서 검증 
- 좋은 성능을 보임 

## 의견
- 마스킹된 이미지 필요 
- Labeled 이미지가 많이 필요하여 Synthetic Image Generator를 사용함 