# MERLOT Reserve: Neural Script Knowledge through Vision and Language and Sound (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Zellers_MERLOT_Reserve_Neural_Script_Knowledge_Through_Vision_and_Language_and_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/zellers2022merlot.png?raw=true"></br>
    <em><font size=2>Reserve architecture.</font></em>
</p>

## 연구목적
- 만약 video와 scripts가 함께 주어진다면, 우리는 자연스럽게 각 장면의 sound를 유추해낼 수 있음
  - Ex) 팝콘을 만드는 영상이라면 냄비가 긁히는 소리, 팝콘이 튀겨지는 소리 등
- 이는 modalities 간의 상관관계가 각 modality의 학습에 사용되는 "learning from reentry"라고 볼 수 있음
- 본 논문에서는 audio, subtitles, vision의 모든 modalities를 사용하여 self-supervised한 방식으로 video를 학습하는 모델을 제안함 (MERLOT Reserve)

## 접근법
- 우선 주어진 video를 non-overlapping segments로 나누고, 각 segment에 대해 vision, text, audio 정보를 추출함
  - Vision: 주어진 segment를 구성하는 모든 프레임 중 중간에 위치한 프레임
  - Text: 주어진 segment의 subtitles
  - Audio: 주어진 segment의 audio signal
- 이때 text는 audio를 자동으로 transcribe하여 생성되므로 두 modalities 간의 정보가 거의 유사하고, 따라서, 학습 시 vision 정보와 함께 text 혹은 audio 정보 중 하나만 제공됨
- Vision 정보는 ViT로, text 정보는 word embedding으로, audio 정보는 Audio Spectogram Transformer로 각각 임베딩하고, BERT에 입력하여 joint modeling 수행
- 학습 방법으로는 contrastive span training을 제안함: audio/text tokens의 25% 가량을 마스킹한 뒤 마스킹된 tokens를 예측하도록 훈련
- 또한, BERT로 입력된 각 segment의 frame tokens와 audio/text tokens를 올바르게 align하는 contrastive loss term도 추가함

## 실험결과
- Fine-tuning된 MERLOT Reserve는 visual commonsense reasoning(VCR) task에서 SOTA 성능을 달성함
- 또한, VQA, activity recognition에서도 좋은 성능을 기록함

## 의견
- /