# Revitalizing CNN Attention via Transformers in Self-Supervised Visual Representation Learning (NIPS, 2021)

[논문 링크](https://proceedings.neurips.cc/paper/2021/hash/21be992eb8016e541a15953eee90760e-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/ge2021revitalizing.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 효과적인 피처 학습은 모델의 분류 성능에 가장 큰 영향을 미침 
- 최근에는 Transformer 구조를 컴퓨터 비전 분야에 적용한 Vision Transformer (ViT) 구조가 널리 사용됨 
- 또한, ViT를 Supervised Learning 방식으로 훈련시키는 것을 넘어 Self-supervised Learning 방식으로 훈련시키려는 연구도 제시되고 있음 
- ViT의 성공은 Transformer의 구조가 CNN 네트워크의 Attention 기능을 효과적으로 향상시킨다는 것을 의미함 
- 하지만, 현재까지의 Attentive CNN은 Supervised Learning 방식으로 훈련되어 왔고, 기존의 Self-supervised Learning 방식은 Attentive CNN을 학습시키는 데 적합하지 않았음 
- 따라서, 본 연구에서는 Transformer 구조를 이용한 Self-supervised Learning 방식인 CNN Attention Revitalization (CARE) 프레임워크를 제안함으로써 Backbone CNN 네트워크의 Attentive한 기능을 증가시킬 수 있는 방안을 제시하고자 함 
- 제안된 방식은 기존의 Contrastive Learning에서 사용하는 Projector, Predictor 구조와 유사한 C-stream과 Transformer 구조를 사용하는 T-stream으로 구성됨 

## 접근법
### (1) CNN-stream (C-stream) 
- C-stream의 구조는 일반적인 Contrastive Learning 구조와 비슷함 
- 두 개의 Projector, 하나의 Predictor 
- SimSiam과 마찬가지로 하나의 이미지로부터 랜덤하게 Augmentation된 두 이미지 간의 유사성을 비교하는 방식으로 훈련 
### (2) Transformer-stream (T-stream) 
- T-stream도 C-stream과 동일하게 CNN Encoder의 출력 Feature Map을 입력으로 받음 
- T-stream은 두 개의 Transformer, 2개의 Projector, 하나의 Predictor로 구성 
- Projector와 Predictor의 구성은 C-stream과 동일 
- 두 개의 Transformer는 같은 아키텍처를 공유
- Transformer는 n개의 연속적인 Attention Block (두 개의 MLP Layer, 하나의 Multi-head Self-attention (MHSA)가 그 사이에 존재) 
- 입력 Feature Map은 Query (q), Key (k), Value (v)으로 구성됨 
- q와 v가 곱해져서 Content-based Attention을 구성
- q와 Position Encoding (p)와 곱해져서 Position-based Attention을 구성 
- 처리가 완료된 T-stream에서도, 두 이미지 간의 유사성을 비교하는 방식으로 훈련 
- C-stream, T-stream 각각의 훈련 외에도 C-stream의 출력과 T-stream의 출력이 유사해지도록 학습하는 Attention Supervision을 추가로 훈련 
### (3) Network Training 
- 최종 Loss Function은 C-stream의 Loss, T-stream의 Loss, Attention Supervision Loss의 합으로 구성됨 
- 학습이 끝나면 CNN Encoder만을 Downstream Task에 이용 

## 실험결과
- 여러 Downstream Tasks에서 좋은 성능을 보임 

## 의견
- Fine-grained Image Classification에도 적용 가능한가?