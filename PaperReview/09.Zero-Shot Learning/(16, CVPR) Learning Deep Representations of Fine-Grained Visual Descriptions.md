# Learning Deep Representations of Fine-Grained Visual Descriptions (CVPR, 2016)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Reed_Learning_Deep_Representations_CVPR_2016_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/reed2016learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Image Understanding의 핵심은 natural language concept와 visual content를 올바르게 연관짓는 것
- 이러한 visual-semantic embeddings의 발전은 zero-shot learning, image captioning과 같은 연구의 발전을 견인함
- 본 연구에서는 fine-grained visual recognition(zero-shot or retriever 포함)을 위해 visual-semantic embeddings을 더욱 강화할 수 있는 higher-capacity text model을 제안하고자 함
- 본 연구는 (1) text model 구성을 위해 CUB와 Oxford-102 flowers 데이터셋의 fine-grained visual descriptions를 수집하였으며, (2) deep neural language model의 end-to-end 학습을 위한 structured joint embedding을 제안하였고, (3) word-based 및 character-based neural language modelss 등 다양한 구조에 대해 평가하였으며, 그 결과 zero-shot recognition과 zero-shot retriever의 SOTA 성능을 개선함

## 접근법
- 이미지와 fine-grained visual descriptions를 동시에 임베딩하는 structured joint embedding은 주어진 description과 매칭되는 이미지와의 compatibility는 최대화하고, 매칭되지 않는 이미지와는 최소화하는 방식으로 진행됨
- 모델의 최종 objective function은 text description으로부터의 클래스 예측과 이미지로부터의 클래스 예측 결과에 대한 classification loss의 합
- 이때 description과 이미지 간의 compatibility를 고려하기 위해 예측값 계산 과정을 세분화
- 먼저 description과 이미지 각각에 learnable encoder function을 적용하여 embedding vector를 추출
- 추출된 embedding vectors 간의 내적값을 최종 예측값 계산에 활용
- 이미지로부터의 최종 예측은 모든 클래스의 description subset을 추출하고, 위의 내적 계산 과정을 거쳤을 때 평균 내적값이 가장 큰 class로 결정
- Description으로부터의 최종 예측은 모든 클래스의 이미지 subset을 추출하고, 위의 내적 계산 과정을 거쳤을 때 평균 내적값이 가장 큰 class로 결정
- 이는 이미지와 텍스트가 같이 학습에 사용되므로 deep symmetric structured joint embedding (DS-SJE)라고 볼 수 있음
- Text encoder 모델로는 Text-based CNN, LSTM과 함께 본 연구에서 제안하는 Convolutional Recurrent Net (CNN-RNN)을 사용하여 각각 평가함

## 실험결과
- CUB와 Oxford-102 flowers 데이터셋에서의 zero-shot recognition 및 retriever 성능 개선

## 의견
- /