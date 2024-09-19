# Deep Contextualized Word Representations (NAACL, 2018)

[논문링크](https://arxiv.org/abs/1802.05365)

<p align="center">
    <img width="600" alt='fig1' src="../img/peters2018deepcontextualizedwordrepresentations.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 사전 훈련된 word representation은 많은 language undestanding 모델의 key components임
- 하지만, 매우 높은 수준의 representation 학습은 어려운데 이는 (1) 구문 및 의미론과 같은 단어 사용의 복잡한 특성과, (2) 이러한 사용이 맥락에 따라 어떻게 달라지는지(다의어)를 모델링해야하기 때문임
- 본 논문에서는 위의 문제를 해결할 수 있는 deep contextualized word representation을 제안하는데, 이는 기존의 모델과 쉽게 결합될 수 있고 여러 challenging한 language understanding 문제에서 높은 성능을 기록함
- 본 논문에서 제안하는 방식은 사전 훈련된 language model에 전체 입력 시퀀스를 넣어 각 단어의 embedding을 구하는 방식 (Embeddings from Language Models (ELMo))
- ELMo와 여타 word embedding model과의 가장 큰 차이점은 각 단어마다 고정된 word embedding을 사용하는 것이 아닌 pre-trained model 자체를 하나의 function 개념으로 사용한다는 것
- 이전의 word2vec 등은 model을 train시킨 뒤 각 word마다의 embedding vector만을 추출해 사용했지만, ELMo는 word마다 embedding vector가 특정되지 않음
- ELMo model을 NLP model의 앞에 연결하는 방식으로 사용

## 접근법
- Two-layer bidirectional LSTM을 사용 (+ layer 사이에 residual connection을 사용)
- Bidirectional LSTM은 k번째 token에 대한 output을 이전의 모든 시퀀스 입력을 통해 계산하는 forward LSTM과 같은 output을 이후의 모든 시퀀스 입력을 통해 계산하는 backward LSTM을 결합한 것
- 두 방향에 대한 log-likelihood를 최대화하는 것을 목적으로 함
- Softmax layer와 token representation에 대한 parameter는 공유되며, forward/backward LSTM의 가중치는 서로 다름 
- k번째 word에 대한 최종 ELMo embedding은 input embedding을 포함한 모든 layer의 intermediate output의 합
- 이때 softmax-normalized weights를 각 layer ouput에 곱하여 더함
- 또한 마지막으로 scale parameter인 gamma를 곱함
- softmax-normalized weights와 scale parameter는 trainable하며 optimization에서 매우 중요함
- 기존 모델에 ELMo를 적용하기 위해 NLP model의 input layer와 다음 layer 사이에 ELMo를 삽입
- 이후 layer에서는 input으로 기존의 input token과 ELMo embedding을 concat하여 사용

## 실험결과
- 단순히 ELMo를 추가하는 것만으로도 baseline model에 비해 성능이 향상됐고, 이를 통해 SOTA를 달성
- ELMo는 syntax 정보를 잘 담고 있으며 다의어 표현에 강점이 있음
- Two-layer bidirectional LSTM에서 층이 낮은 layer(input layer에 가까운 layer)일수록 syntax 정보를, 층이 높은 layer(output layer에 가까운 layer)일수록 semantic 정보를 저장

## 의견
- /