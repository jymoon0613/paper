# DeViSE: A Deep Visual-Semantic Embedding Model (NIPS, 2013)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2013/hash/7cce53cf90577442771720a370c3c723-Abstract.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/frome2013devise.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이전의 object classification 모델들은 주어진 object를 $N$개의 discrete하게 구분된 class 중 하나로 분류하도록 훈련됨
- 하지만 이러한 방식은 모델이 학습한 semantic information이 훈련 중에는 고려하지 않았던 unseen object에 대해서는 잘 transfer되지 않는다는 문제가 있음
- 이러한 문제를 해결하기 위해 인공적으로 구분된 object categories에 대해 모델을 훈련시키는 것이 아니라 visual space의 연속성과 연관성을 고려하여 모델을 훈련시키는 것임
- 따라서, 본 논문은 분류 모델이 기존의 방식처럼 labeled images로부터 학습할 뿐만 아니라 unannotated text data로부터 semantic information을 학습하도록 하는 방법을 제안함 (deep visual-semantic embedding, DeViSE)
> - DeViSE는 textual data로부터 labels 간의 연관성을 학습하고 주어진 images를 semantic embedding space로 매핑함

## 접근법
- 본 논문의 핵심 아이디어는 text domain에서 학습한 semantic knowledge를 object recognition model로 transfer하는 것임
- 이를 위해, 먼저 간단한 neural language model을 pre-training하여 semantically-meaningful하고 dense한 word representations를 학습시킴
> - Skip-gram text model을 wikipedia text corpus에서 학습시킴
- 동시에 object recognition model도 훈련시킴
> - CNN 모델을 ImageNet에서 학습시킴
- 이후 훈련된 object recognition model을 pre-trained language model이 image label로부터 생성하는 vector representation을 예측하도록 재훈련시킴으로써 deep visual-semantic model을 construct함
> - 학습된 language model은 image lables를 embedding vectors로 매핑함
> - Object recognition model의 기존 softmax output layer는 output features를 embedding vector와 같은 dimension으로 projection하는 fully-connected layer로 대체됨
> - 최종적으로 model은 langauge model이 매핑한 label embedding vectors를 예측하도록 재훈련(fine-tune)됨

## 실험결과
- DeViSE는 zero-shot scenario에서 좋은 성능을 기록함
> - Pre-trained language model은 주어진 unseen image의 label을 embedding vector로 매핑함
> - Recongition model은 주어진 unseen 이미지에 대한 label embedding vector를 계산함
> - Embedding space에서 예측된 label embedding vector와 가장 가까운(유사한) embedding을 지니는 class로 해당 unseen image를 분류함

## 의견
- / 