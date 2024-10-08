# TransFER: Learning Relation-Aware Facial Expression Representations With Transformers (ICCV, 2021)

[논문 링크](https://openaccess.thecvf.com/content/ICCV2021/html/Xue_TransFER_Learning_Relation-Aware_Facial_Expression_Representations_With_Transformers_ICCV_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/xue2021transfer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- FER에 대한 관심이 커져가고 있으며, 어느 정도의 성능이 나왔지만 여전히 여러 문제가 있음 (Large Inter-class similarities, Small Inter-class Similarities)
- 현재까지의 FER 방법론은 Global Feature-based와 Local Feature-based로 구분할 수 있음 
- Global Feature-based는 전체 이미지에서 한 번에 분류를 수행하지만, 이는 클래스 구분에 매우 중요한 여러 Local Features를 놓칠 수 있는 위험이 있음 
- Local-based 방법 중 Landmark 기반의 방법은 Facial Landmark 중심의 Crop을 기반으로 분류를 수행하지만, 모든 이미지마다 Facial Landmark의 위치는 다르므로 사전에 정의할 수 없고, Facial Landmark Detector를 사용하더라도 결과가 부정확할 수 있음 
- Local-based 방법 중 Attention 기반의 방법은 효과적이지만, 중요한 피처가 아닌 불필요한 부분에 집중하는 문제가 있을 수 있음 
- 따라서, 본 논문에서는 FER의 성능 개선을 위해 서로 다른 로컬 패치 간의 관계를 전역적 범위에서 탐색하여 중요한 패치를 강조하고, 쓸모 없는 패치를 억제하기 위한 Trans-FER 모델을 제안함 

## 접근법
### (1) Local CNNs 
- CNN Backbone이 기본적인 Feature를 추출 
- 이후 여러 개의 Spatial Attention Module을 사용하여 모델이 이미지의 여러 Discriminative한 Region에 집중하도록 함 
- 하지만, Attention 시에 별도의 처리가 없으면 모델은 중복되는 부분에 집중하거나 Occlusion과 같은 불필요한 Region에 집중할 수 있음 
- 따라서 Multi-Attention Dropping (MAD)를 사용하여 제약을 줌 
### (2) Multi-Attention Dropping (MAD) 
- Feature Map이 주어지면, 정해진 비율만큼 무작위 추출된 채널을 0으로 마스킹함 
- 처리된 결과에 Max 연산을 적용하고, 이를 Attention Mask로 사용하여 입력 Feature Map과 Point-wise Multiplication함 
### (3) Multi-head Self-Attention Dropping (MSAD) 
- MAD를 거친 Feature Map에서, 서로 다른 로컬 패치 간의 관계를 파악하기 위해 ViT 구조를 사용함 
- Mult-head 중 일부를 마스킹하여 제약을 줌 

## 실험결과
- RAF-DB, AffectNet, FERPlus 데이터셋 사용 

## 의견
- /