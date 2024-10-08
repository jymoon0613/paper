# Attention Is All You Need (NIPS, 2017)

[논문링크](https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/vaswani2017attention.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- RNN(LSTM, GRU) 구조는 여러 NLP tasks에서 뛰어난 성능을 기록함
- 하지만 recurrent한 처리 방식은 병렬처리가 불가하다는 점과 sequence가 길어지면 메모리 부담이 커진다는 문제가 있음
- RNN의 연산 효율성을 높이기 위한 여러 방법이 제안되었지만 suquential computation이라는 근본적인 문제는 여전히 해결되지 못함
- 입력/출력 sequence와 상관없이 직접적인 연관성을 모델링하는 attention mechanism은 다양한 tasks에서 필수적인 요소로서 사용됨
- 하지만, 이러한 attention mechanism은 거의 모든 경우 RNN의 일부로서 함께 사용되고 있음
- 본 논문에서는 recurrent한 연산을 제거하는 대신 attention mechanism에 전적으로 의존하는 모델 아키텍처인 Transformer를 제안함
- Transformer는 훨씬 더 강력한 병렬화를 지원하며 짧은 훈련시간으로도 뛰어난 번역 성능을 기록함

## 접근법
- 대부분의 neural sequence 모델은 encoder-decoder 구조를 사용함
- Encoder는 입력 시퀀스로부터 continuous한 representations를 매핑함
- Decoder는 encoder의 출력 representations을 참조하여 output 시퀀스를 하나씩 출력함
- 이때 매 time-step의 decoder 출력은 다음 step에서 다시 encoder의 입력으로 사용됨
- Transformer도 전체적으로 위와 같은 아키텍처를 따름

### (1) Encoder and Decoder Stacks
- Encoder는 동일한 모양의 레이어가 N=6개 쌓인 구조임
- 각 레이어는 multi-head self-attention과 position-wise fully connected network로 구성됨
- 두 모듈 각각에 residual connection 및 layer normalization을 적용함
- Decoder도 동일한 모양의 레이어가 N=6개 쌓인 구조임
- Decoder의 각 레이어는 encoder stack의 output에 multi-head attention을 적용하는 모듈이 추가된다는 점을 제외하면 encoder stack과 동일함
- Decoder의 각 모듈에도 residual connection 및 layer normalization을 적용함
- 또한 decoder의 self-attention은 해당 output sequence의 이전 positions에 대한 정보만을 사용할 수 있도록 masking 과정을 추가함
  
### (2) Attention
- Attention function은 query와 일련의 key-value 쌍을 output에 매핑시키는 작업
> - Query: 영향을 받는 단어(A)를 나타내는 변수
> - Key: 영향을 주는 단어(B)를 나타내는 변수
> - Value: 그 영향에 대한 가중치를 표현
- 논문에서는 이러한 self-attention을 다중으로 구현한 multi-head attention 사용 (본 논문에서는 h=8)
- Multi-head self-attention은 여러 개의 subspaces 상에서 각각 self-attention을 수행하므로 다양한 측면의 representation을 학습할 수 있다는 장점이 있음
- 각 head에서 나온 output을 최종적으로 concat함
- 이러한 기본적인 self-attention과 함께 decoder에서는 약간 다른 형태의 self-attention이 사용됨
> - Query는 직전 decoder layer에서 계산된 것을 사용하고 key와 value는 사전에 저장된 encoder의 것을 사용하여 decoder의 모든 position에서 input sequence 전체에 attention할 수 있도록 하는 encoder-decoder attention
> - self-attention은 한 position에 대해 해당 position과 그 이전 position 정보만 사용할 수 있도록 masking되어 적용됨 (auto-regressive한 성질을 유지하기 위해)

### (3) Position-wise Feed-Forward Networks
- Encoder와 decoder layer 각각은 fully connected feed-forward network(FFN)를 포함함
- FFN은 모든 position에 대해 각각 적용되며 가중치는 공유됨

### (4) Positional Encoding
- Transformer는 recurrent한 연산을 하지 않으므로 sequence에 대한 정보를 포함하기 위한 trick이 필요함
- 따라서 positional encoding 기법으로 각 position(단어)의 상대적인 위치 정보를 포함하도록 함
- 기존 embedding과 같은 차원의 positional encoding을 만들어 더해줌으로써, Transformer는 time signal을 가진 embedding을 입력으로 사용할 수 있음
- 본 논문에서는 각 position의 상대적인 정보를 표현하고, 선형 변환이 가능한 방식을 구현하기 위해 positional encoding function으로 sinusoidal version을 사용함 (서로 다른 frequency를 갖는 sin 및 cosine functions)

## 실험결과
- Transformer는 훨씬 짧은 훈련시간으로도 machine translation tasks에서 ensemble 모델을 포함한 이전의 방법들을 능가함

## 의견
- /