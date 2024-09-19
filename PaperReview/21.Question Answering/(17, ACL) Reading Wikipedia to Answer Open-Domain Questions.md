# Reading Wikipedia to Answer Open-Domain Questions (ACL, 2017)

[논문링크](https://arxiv.org/abs/1704.00051)

<p align="center">
    <img width="500" alt='fig1' src="../img/chen2017reading.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 Wikipedia를 knowledge source로 한 open-domain QA에 관해 다룸 (DrQA)
- 이를 위해 여러 Wikipedia 문서 중 가장 관련있는 문서를 retrieve해야 하며, 그 문서를 scan하면서 올바른 답을 식별해야 함
> - Task를 machine reading at scale (MRS)이라고 정의함
- DrQA는 두 개의 sub-model로 구성됨
> - Document Retriever: 적절한 문서 반환
> - Document Reader: 반환된 문서에서 answer span을 찾음

## 접근법
- non-machine learning의 document retrieval system을 사용하여 연관된 article을 추출함
- Document reader는 bi-directional LSTM을 사용하여 answer span을 예측
> - RNN의 입력 feature vector는 3개의 요소로 구성됨
> - (1) Glove word embedding
> - (2) Exact match binary features: 현재 단어가 question에 있는 단어인지를 0/1로 알려주는 feature
> - (3) Token features: part-of-speech(POS), named entity recognition(NER) tags, (normalized) term frequency(TF)로 이루어진 manual features
> - 3개의 요소가 concat되어 한 단어의 feature vector가 됨
> - 입력 feature vector를 bi-directional LSTM으로 처리하고, 마찬가지로 embedding된 question과 연결하여 start, end의 answer span을 예측

## 실험결과
- SQuAD, Wikipedia 모두에서 좋은 성능 기록

## 의견
- / 