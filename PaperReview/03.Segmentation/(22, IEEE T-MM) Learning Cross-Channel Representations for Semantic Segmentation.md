# Learning Cross-Channel Representations for Semantic Segmentation (IEEE T-MM, 2022)

[논문링크](https://ieeexplore.ieee.org/abstract/document/9713684)

<p align="center">
    <img width="800" alt='fig1' src="../img/ma2022learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Spatial contextual information 추출에 집중했던 이전의 semantic segmentation 연구들과 달리 본 논문은 channel information에 집중함
- Feature extraction stage에서, informative channels는 강조하고 불필요한 channels를 억제하여 더 valuable한 semantic features를 추출함
> - Comprehensive information channel attention(CiCA) module을 backbone에 추가하여 features를 점진적으로 re-weight함
> - CiCA는 각 channel에 frequency domain analysis를 적용하여 high/low frequency components를 추출함
> - 이를 바탕으로 각 channel의 global/detailed information을 학습한 뒤, 각 channel의 중요도를 식별하여 informative한 channels를 강조함
- 또한, Prediction stage에서, channels 간의 연결 및 상호보완을 강화하여 object에 대한 더 정확한 예측을 촉진함 
> - InterChannel relationship reasoning(iCRR) module을 backbone 최상단에 추가함
> - iCRR은 channels 간의 의존성과 상호보완적인 관계를 모델링하고, 이를 바탕으로 features를 업데이트함

## 접근법
- 제안하는 모델은 FCN 기반의 아키텍처를 사용함
- CiCA module은 모든 residual block에 추가됨
> - 마지막 1x1 convolution layer 이후에 추가되어 점진적으로 feature channels를 재보정함
- CiCA module은 각 channel의 global/detailed information을 학습하여 informative channels를 강조함
> - 먼저 CiCA module에 입력된 feature maps은 row-wise/column-wise sum을 통해 horizontal/vertical vector로 변환됨
> - 두 vectors에 대해 1D discrete wavelet transform(DWT)를 적용하여 projection함
> - 이후 1D convolution 및 sigmoid를 적용하여 두 vectors로부터 channel attention weights을 계산함
> - 이후 두 attention weights를 적용하여 feature maps를 보정함
- 추출된 features는 final segmentation mask를 예측하기 위해 prediction stage로 입력되며, 이때 prediction stage는 iCRR module과 몇 개의 convolutional layers로 구성됨
- iCRR module은 feature maps의 channels 간 의존성과 상호보완성을 식별하여 pixel-wise refinement를 적용함
> - Graph reasoning을 사용하여 각 channel을 graph의 node로 치환하고 channels 간의 연관성을 추론함
> - Feature maps을 graph space로 projection함
> - 이때 채널 개수 만큼의 node(or node features)가 존재함
> - 모든 node features를 서로 다른 두 transform matrices를 적용하여 projection 한 뒤, 두 matrices의 내적을 계산하여 node 간 impact coefficients을 계산
> - Normalization을 적용한 impact coefficients를 통해 node features를 scaling함
> - Graph space projection에 사용되었던 weights를 재사용하여 node features를 다시 원래의 feature space로 projection함

## 실험결과
- Cityscapes, ADE20K, PASCAL Context에서 좋은 성능 기록

## 의견
- / 