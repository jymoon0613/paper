# Online Knowledge Distillation via Collaborative Learning (CVPR, 2020)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Guo_Online_Knowledge_Distillation_via_Collaborative_Learning_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/guo2020online.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Knowledge Distillation (KD)는 주로 Teacher-Student의 구조의 학습 방식을 지님 
- 학습의 대상이 되는 Student Network는 초기화되어 처음부터 학습하는 것보다 Teacher Network의 구조화된 정보를 바탕으로 학습하는 것이 성능 향상에 더욱 효과적임 
- 하지만, 현재의 KD 방식들은 모두 사전 훈련된 Teacher Network가 필요함, 즉, 학습 정보는 Teacher에서 Student로 한 방향으로만 흐름 
- Online Distillation은 오직 Student Network만을 고려하는 방식으로, Teacher Network에 대한 사전 학습 없이 서로가 서로의 정보를 학습하는 방식임 
- 본 논문에서는, Knowledge Distillation Method via Collaborative Learning (KDCL) 방식을 사용하여 서로 다른 구조의 Student Network들이 서로의 정보를 공유하며 협력적으로 학습하는 방법을 제안함 
- Knowledge Distillation: 사전 학습된 큰 네트워크 (Teacher Network)의 구조화된 지식을 비교적 작은 Student Network가 모방하여 학습 
- Collaborative Learning: 오직 여러 Student Network로만 구성된 학습 환경에서, 각 Student Network는 자신의 학습 정보를 서로 공유하며 협력적으로 학습 (Ensemble과 유사) 

## 접근법
- KDCL은 따로 Teacher Network를 두지 않고, Student Network들의 출력을 기준으로 Soft Target을 Online 방식으로 생성함 
- 모든 Student Network의 Output 평균을 Soft Target으로 사용 

## 실험결과
- 적은 파라미터 수로 좋은 성능 달성 

## 의견
- 간단하지만 효과적인 Knowledge Distillation (Collaborative Learning) 방식 제안 