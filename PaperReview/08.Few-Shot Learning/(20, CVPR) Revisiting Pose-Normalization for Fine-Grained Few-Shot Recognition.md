# Revisiting Pose-Normalization for Fine-Grained Few-Shot Recognition (CVPR, 2020)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Tang_Revisiting_Pose-Normalization_for_Fine-Grained_Few-Shot_Recognition_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/tang2020revisiting.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Few-shot 상황을 가정함: 소수의 클래스 이미지만을 가지고 해당 클래스에 대한 분류를 수행
- 인간은 few-shot 상황에서도 인지 능력이 뛰어나지만, machine의 경우 그렇지 않은 경우가 많음 
- 이러한 few-shot 상황에서의 machine vision의 한계는 labeled 데이터 수집이 힘든 Fine-Grained Visual Classification (FGVC)에서 문제가 될 수 있음
- 인간은 spatail deformations에 invariant한 피처를 학습하는 경향이 있음 (인간이 새의 종류를 구분할 때 날개의 색깔, 부리의 모양 등 여러 local parts의 특징을 종합적으로 고려함)
- 이러한 인간 인지의 특징은 배경, 카메라 각도 등에 무관하게 일관적인 클래스 간 유사성/차이 인식을 가능하게 함
- 이러한 접근방식을 차용하는 것은 "pose-normalized"라고 불리는 방법들로서 딥러닝이 등장하기 이전 FGVC에서 주로 사용되던 방식이었음
- 하지만 최근의 딥러닝 방법들이 pose-normalized 방식을 사용하지 않는 것은 대부분의 연구가 일반적인 supervised classification에 맞춰져 있기 때문임
- 이러한 setting에서는 예측해야 할 클래스를 기본적으로 알고 있다고 가정하며, 학습을 위한 충분한 데이터가 주어지기 때문에 pose와 background에 대한 고려가 불필요함
- 또한, 학습된 표현은 완전히 새로운 클래스에 적용되지는 않기 때문에 오히려 각 class에 biased되어 있는 pose와 background 정보를 활용하는 것이 분류에 더 효과적일 수 있음
- 하지만, 현재 가정하고 있는 few-shot 상황에서는 (모델은 데이터가 제한적인 새로운 클래스에 적용되어야 함) pose-normalized 방식이 더 효과적일 수 있음
- 따라서, 본 논문에서는 few-shot FGVC를 위해 pose-normalized 방식을 적용하는 방법을 제안하고 왜 효과가 있는지를 설명하고자 함
- 이러한 pose-normalized 방식은 구현이 간단하며, few-shot 상황에서 성능을 크게 개선함

## 접근법
- FGVC에서는 클래스 간 subtle difference를 포착하는 것이 어렵고 이는 labeled 데이터가 적은 few-shot 상황에서 더욱 어려움
- 따라서, pose normalization을 통해 이미지의 가장 informative한 부분에 focus하도록 하는 것이 중요함
- 모델 training 시에 part annotations 사용함
- 모델은 part annotations을 사용하여 pose를 예측하고 이는 few-shot classification loss와 같이 훈련됨
- Inference 시에는 part annotations 없이 예측된 pose heatmaps를 기반으로 추론함

## 실험결과
- CUB, FGVC-Aircraft datasets으로 평가함
- 여러 baseline과 비교
- Pose normalization은 일관적으로 성능 향상을 가져옴

## 의견
- Part annotations를 사용하므로 우리가 하려는 방법과는 차이가 있음
- 하지만 FGVC에서 pose-invariant한 local features가 얼마나 중요한지 증명한다는 점에서 참고할만함