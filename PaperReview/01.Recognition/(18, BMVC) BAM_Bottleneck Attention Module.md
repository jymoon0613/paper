# BAM: Bottleneck Attention Module (BMVC, 2018)

[논문 링크](https://arxiv.org/abs/1807.06514)

<p align="center">
    <img width="600" alt='fig1' src="../img/park2018bam.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN 아키텍처와 쉽게 결합될 수 있는 Attention Module을 제안함 
- Attention Module을 통해 네트워크의 표현력을 강화 

## 접근법
### (1) Bottleneck Attention Module 
- Channel Attention과 Spatial Attention으로 구분됨 
- Channel Attention과 Spatial Attention 수행으로 계산된 3D Attention Map을 입력 Feature Map과 Pointwise Multiplication하고, 결과를 입력 Feature Map과 더함 
### (2) Channel Attention Branch 
- 채널 간에 존재하는 중요도를 스케일링 하기 위해 Global Average Pooling 진행 (C * 1 * 1) 
- GAP의 결과는 각 채널의 핵심 정보를 의미함 
- 이후 Dense Layer와 Batch Normalization을 거침 
### (3) Spatial Attention Branch 
- 각 Feature Map 상의 위치 중요도를 스케일링 하기 위해 진행 
- Receptive Fields를 확장하기 위해 Dilated Convolution 사용 
- 두 번의 1X1 Convolution과 3X3 Convolution을 진행 + Batch Normalization 

## 실험결과
- ImageNet, COCO 등의 데이터셋에서 성능 검증 

## 의견
- 두 개의 Branch를 하나로 통합? 