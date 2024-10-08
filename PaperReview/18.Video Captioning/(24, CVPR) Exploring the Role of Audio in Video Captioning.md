# Exploring the Role of Audio in Video Captioning (CVPR, 2024)

[논문링크](https://openaccess.thecvf.com/content/CVPR2024W/MULA/html/Shen_Exploring_the_Role_of_Audio_in_Video_Captioning_CVPRW_2024_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/shen2024exploring.png?raw=true"></br>
    <em><font size=2>Overview of our audio-visual video captioning framework.</font></em>
</p>

## 연구목적
- Video captioning 성능 향상을 위해 대규모 비디오와 이에 대응하는 text transcript를 supervised text signals로 pre-training하는 방법이 최근 많이 쓰이고 있음
- 하지만 본 논문에서는 transcript가 오디오의 오직 일부 정보만을 담고 있으며 audio modality 자체를 end-to-end로 학습하는 것이 더 효과적이라고 주장함
- 따라서, 본 논문은 audio modality를 활용하여 video captioning 모델을 pre-training 시키고, 이를 더 효과적으로 만들기 위한 modality balanced pre-training (MBP) loss 및 새로운 cross-modal fusion 모듈을 제안함

## 접근법
- 본 논문에서 제안하는 audio-visual video captioning framework는 video encoder, audio encoder, cross-modal encoder, caption decode로 구성됨
- Video encoder(Video Swin Transformer)는 입력 비디오로부터 video embeddings를 추출하고, audio encoder(Audio Spectogram Transformer)는 입력 오디오 spectogram으로부터 audio embeddings를 추출함
- Cross-modal encoder는 두 embeddings로부터 cross-modal embeddings를 모델링하고, 마지막으로 caption decoder는 audio-visual embeddings를 입력받아 caption을 autoregressive하게 생성함
- 하지만, output captions에 대해서만 loss를 계산하여 학습하는 경우 모델은 거의 대부분 audio 정보만을 활용하여 예측하도록 훈련됨
- 따라서, MBP loss를 설계하여 두 modality 사이의 밸런스를 맞춰줌
  - Video-only prediction과 audio-only prediction 각각에 대한 loss도 계산하고 학습
  - 기존의 multi-modal loss와 함께 새롭게 계산된 두 loss를 적절히 가중해가며 학습 수행
- 또한, audio 및 visual embeddings를 fusion하기 위해 cross-modal encoder로 local-global fusion을 수행함
  - 각 modality 간의 self/cross-attention

## 실험결과
- 제안하는 방법으로 훈련하는 경우 여러 video captioning 벤치마크에서 SOTA 성능 기록

## 의견
- /