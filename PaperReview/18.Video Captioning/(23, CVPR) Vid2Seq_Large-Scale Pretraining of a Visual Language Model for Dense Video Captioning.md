# Vid2Seq: Large-Scale Pretraining of a Visual Language Model for Dense Video Captioning (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Yang_Vid2Seq_Large-Scale_Pretraining_of_a_Visual_Language_Model_for_Dense_CVPR_2023_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2023vid2seq.png?raw=true"></br>
    <em><font size=2>Vid2Seq model overview.</font></em>
</p>

## 연구목적
- Dense video captioning은 짧은 video clip에 대한 하나의 caption을 생성하는 standard video captioning과 달리 주어진 untrimmed video에 모든 events에 대한 caption을 생성하는 task임
  - Dense video captiong은 수 분에 이르는 영상에서 모든 events를 temporal하게 localizing해야 한다는 점에서 더 어려움
  - Video 내의 서로 다른 events 간의 relationships를 모델링하는 것이 중요함
- 본 논문에서는 dense video captioning을 위한 Vid2Seq 모델을 제안함
- Vid2Seq는 video와 transcribed speech가 주어졌을 때 caption tokens와 time tokens로 구성된 single token sequence를 예측함
  - Time tokens는 event의 시작과 끝을 나타냄
- Unlabeled된 대규모 narrated videos와 각각의 transcribed speech를 사용하여 Vid2Seq를 훈련시킴

## 접근법
- Vid2Seq 모델은 multi-modal encoder-decoder 아키텍처로 구성됨
- Vid2Seq encoder는 video frames와 transcribed speech seqeunce를 입력으로 받음
- 이때 transcribed speech에 대한 text tokens와 함께 video 내 상대적인 timestamps를 나타내는 time tokens를 같이 입력함
  - 각 Transcribed speech는 시작 time token, 종료 time token, text tokens 형식으로 입력됨
- Vid2Seq decoder은 sequence-to-sequence 방식으로 event seqeuence를 예측하며, 이때 event seqeuence를 구성하는 각 event는 textual description과 timestamps로 구성됨
  - Event는 시작 time token, 종료 time token, text tokens로 구성됨
- Vid2Seq 모델을 대규모 unlabeled narrated videos로 pre-training함
- Unlabeled narrated videos은 dense event captioning annotations가 없으므로 transcribed speech sentences와 그 timestamps를 supervisory signal로 사용함
- 또한, speech transcripts가 항상 visually grounded되는 것은 아니며 자주 temporally misaligned되기 때문에 해당 데이터를 통한 훈련은 weak supervision만 제공함
- 그럼에도 불구하고, speech sequence가 manually annotated event sequence와 형식/내용 측면에서 유사하고, 서로 다른 speech segments간 long-term relations를 학습할 수 있다는 점에서 효과적임

## 실험결과
- Vid2Seq는 dense video captioning 및 standard video captioning tasks에서 SOTA 성능 달성
- Event localization, video paragraph captioning에서도 좋은 성능 기록

## 의견
- /