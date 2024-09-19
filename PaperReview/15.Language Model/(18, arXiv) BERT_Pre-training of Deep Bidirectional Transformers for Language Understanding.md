# BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding (arXiv, 2018)

[논문링크](https://arxiv.org/abs/1810.04805)

<p align="center">
    <img width="600" alt='fig1' src="../img/devlin2018bert.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Pre-training은 여러 NLP tasks에서 좋은 성능을 가져다줌
- ELMo는 사전 훈련된 network와 task-specific한 network를 연결함 (feature-based)
- GPT는 사전 훈련된 network에 소수의 layer를 추가하여 task-specific한 parameters를 학습함 (fine-tuning)
- 두 방법의 공통점은 사전 학습시 shallow bidirectional(각각의 단방향 concat) 혹은 unidirectional한 language model을 사용한다는 것
- 하지만 unidirectional한 pre-train은 QA와 같은 task 등에서 매우 제한적인 성능을 보장할 수 있음
- 따라서, 본 논문에서는 Transformer 구조 기반의 bidirectional한 representations을 학습하는 아키텍처를 제안함 (BERT)

## 접근법
- BERT는 Transformer encoder 구조를 사용함
- GPT는 Transformer decoder 구조를 사용하므로 unidirectional한 self-attention을 사용하지만 BERT는 Transformer encoder 구조를 사용하므로 bidirectional한 self-attention을 사용함
- 여러 tasks에 적용가능하도록 BERT의 입력 구조를 설정함: WordPiece embeddings 사용, 가장 첫 token은 CLS token, 두 문장이 pair로 들어올 때는 SEP token을 추가하여 구분하고 모든 token에 대해 sentence A 또는 B에 속하는지를 나타내는 learned embeddings를 더해줌
- Pre-train tasks로 (1) Masked Language Model(Masked LM)과 (2) Next Sentence Prediction(NSP) 사용
- 데이터셋으로는 BooksCorpus와 Wikipedia 데이터셋을 사용함

### (1) Masked LM
- Bidirectional한 representation을 학습하기 위해 input token의 일부를 random하게 마스킹하고, 마스킹된 token을 예측하도록 함 (15% 마스킹함)
- 하지만 마스킹되는 위치에 별도의 MASK token을 대체하게 되면 해당 token은 fine-tuning시에는 사용되지 않으므로 mismatch가 생김
- 따라서, 항상 MASK token으로 대체하는 것이 아니라 15%의 마스킹되는 token중 (1) 80%는 MASK token으로 대체, (2) 10%는 random한 token으로 대체, (3) 10%는 대체하지 않고 유지함 

### (2) NSP
- Question Answering(QA)나 Natural Language Inference(NLI)와 같은 tasks는 두 문장 간의 관계를 이해하는 것이 중요함
- 문장 간의 관계를 학습하기 위해 NSP task를 수행
- 두 문장을 샘플링하고 이때 50%의 경우 문장 B는 실제로 문장 A에 뒤따르는 문장이며 나머지 50%는 서로 관련없는 문장 
- NSP와 Masked LM을 동시에 진행

### (3) Fine-tuning
- 사전 훈련이 끝나면 task specific한 layer 하나와 입력 구조의 변경만으로도 여러 tasks에 쉽게 적용 가능함 (새로 추가되는 가중치가 매우 적음)
- 또한, fine-tuning을 사전 훈련에 비해 매우 빠름

## 실험결과
- 11개의 NLP tasks에서 SOTA 성능 달성
- Masked LM 혹은 NSP를 하나라도 제거하면 성능이 크게 감소함
- 특히 NSP을 제거하는 경우 NLI 계열의 tasks에서 성능이 크게 하락하는데 이는 NSP task가 문장 간의 논리적인 구조 파악에 매우 중요한 역할을 하고 있음을 나타냄
- Masked LM 대신 단방향의 language model로 모델을 훈련시켰을 때 성능이 하락하였으며 이는 ELMo와 같이 bidirectional LSTM을 사용했을 때도 마찬가지임: Masked LM이 bidirectional representation을 학습하는 데 훨씬 효과적
- 모델의 크기가 커질수록 성능은 일관적으로 향상됨
- Fine-tuning 방법이 아니라 task-specific한 netowkr를 연결하는 feature-based 방법을 사용해도 효과적이었음

## 의견
- /