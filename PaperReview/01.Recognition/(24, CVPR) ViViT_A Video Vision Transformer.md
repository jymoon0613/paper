# ViViT: A Video Vision Transformer (CVPR, 2024)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Arnab_ViViT_A_Video_Vision_Transformer_ICCV_2021_paper.html?ref=https://githubhelp.com)

<p align="center">
    <img width="600" alt='fig1' src="../img/arnab2021vivit.png?raw=true"></br>
    <em><font size=2>We propose a pure-transformer architecture for video classification, inspired by the recent success of such models for images.</font></em>
</p>

## 연구목적
- Video classification을 위한 pure-Transformer 아키텍처를 제안함

## 접근법
- Video 입력을 임베딩하는 방법으로는 (1) 일부 프레임을 uniform하게 샘플링하여 각 프레임별로 2D 임베딩을 진행하는 방법과, (2) 전체 프레임을 t개의 그룹으로 묶어 spatial-temporal하게 임베딩하는 방법을 사용함
- 기존의 self-attention을 spatial self-attention과 temporal self-attention을 분할함
    - Spatial self-attention은 같은 timestep에 존재하는 tokens 간의 interactions를 모델링함
    - Temporal self-attention은 전체 timestep에서 동일한 2D position에 위치하는 tokens 간의 interactions를 모델링함
    - 실제 구현에서는 multi-heads를 이등분하고 spatial/temporal self-attention을 반반씩 연산한 뒤 하나의 attention output으로 매핑함
- Image 도메인에서 pre-trained된 weights를 사용하되, video domain에 맞게 적절히 변형하여 사용함

## 실험결과
- Video classification task에서 SOTA 달성

## 의견
- /