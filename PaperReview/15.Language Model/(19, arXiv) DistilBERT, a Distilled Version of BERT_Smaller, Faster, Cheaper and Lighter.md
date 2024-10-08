# DistilBERT, a Distilled Version of BERT: Smaller, Faster, Cheaper and Lighter (arXiv, 2019)

[논문링크](https://arxiv.org/abs/1910.01108)

<p align="center">
    <img width="600" alt='fig1' src="../img/sanh2019distilbert.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근 NLP에서는 매우 큰 스케일의 Pre-trained Language Model이 사용되고 있음
- 이러한 방식은 모델의 computational requirements를 충족시키기 위한 비용이 크고, real-time on-device 활용이 어렵다는 단점이 존재함
- 따라서, 본 논문에서는 지식 증류 기법을 사용하여 더 작은 모델로도 거대한 모델과 유사한 성능을 달성하도록 함 (DistilBERT)

## 접근법
- Student architecture인 DistilBERT는 기존 BERT의 구조를 유지하되 레이어 수를 줄임
- Teacher architecture로는 기존의 BERT 모델을 사용
- BERT가 사전 훈련되는 corpus를 똑같이 사용하여 distillation 진행

## 실험결과
- DistilBERT는 기존의 BERT보다 40% 크기가 감소했으며 (파라미터 수 기준), 60%더 빠른 처리가 가능하게 됨
- 그러면서도 기존 BERT 성능의 97%를 유지
- 즉, 더 작은 크기로도 비슷한 성능을 달성함

## 의견
- /