# Towards Semi-Supervised Deep Facial Expression Recognition With an Adaptive Confidence Margin (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Li_Towards_Semi-Supervised_Deep_Facial_Expression_Recognition_With_an_Adaptive_Confidence_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/li2022towards.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- FER은 Visual Emotion을 이해하는 작업임 
- Large Scale FER Labels를 수집하는 것은 비용이 크고 어려움 
- 따라서, Labels 의존도가 낮은 학습 방법이 필요함 (본 연구에서는 Semi-supervised Learning) 
- Label 의존도가 낮은 Semi-supervised Learning을 사용하되, Unlabeled Data에 대한 활용도를 극대화하기 위해 Adaptive Confidence Margin (Ada-CM) 방법을 제안함 

## 접근법
- 기존의 semi-supervised learning에서 주로 사용되었던 pseudo-label 방식은 모든 클래스에 대해 일관적인 confidence margin을 사용함
- 본 연구에서는 감정 클래스별로 adaptive한 confidence margin 사용
- Confidence margin 보다 높은 confidence score를 기록한 unlabeled 데이터는 해당하는 클래스의 pseudo-label을 사용하여 일반적인 분류 학습
- Confidence margin 보다 낮은 confidence score를 기록한 unlabeled 데이터는 서로 다른 weakly augmented view로 변환되어 피처 수준의 contrastive learning에 사용됨
- 즉, 낮은 confidence score를 기록한 데이터를 사용하지 않는 기존의 방법에 비해, 제안하는 Ada-CM은 학습 과정에서 모든 unlabeled 데이터를 사용함
  
## 실험결과
- 여러 FER Benchmark 데이터셋에서 검증 (RAF-DB, SFEW, AffectNet)
- 다른 semi-supervised learning 방법의 성능 개선
  
## 의견
- Semi-supervised learning 방식도 label-dependency 줄이는 데 효과적