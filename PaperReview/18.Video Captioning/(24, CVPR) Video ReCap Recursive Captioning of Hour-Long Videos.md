# Video ReCap: Recursive Captioning of Hour-Long Videos (CVPR, 2024)

[논문링크](https://openaccess.thecvf.com/content/CVPR2024/html/Islam_Video_ReCap_Recursive_Captioning_of_Hour-Long_Videos_CVPR_2024_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/islam2024video.png?raw=true"></br>
    <em><font size=2>The Video ReCap model.</font></em>
</p>

## 연구목적
- 최근 대부분의 video captioning 모델들은 5-15초 이내의 짧은 비디오 클립만을 다루도록 설계됨
- 본 논문에서는 긴 비디오 입력을 다루기 위해 hierarchical한 방식으로 캡션을 생성하는 방법을 제안함
    1. 수 초 이내의 짧은 비디오 클립에 대한 캡션 생성
    2. 하위 캡션들을 중간 길이의 descriptions로 통합
    3. 중간 길이의 descriptions를 포괄적으로 최종 요약 
- 하지만 이러한 hierarchical video captioning 모델을 구현하는 과정에서 다음과 같은 어려움이 발생할 수 있음
    1. 모델은 수 초 이내에서 몇 시간에 이르는 다양한 길이의 비디오 입력을 다룰 수 있어야 함
    2. Long-range 비디오는 상당히 redundant하기 때문에 모델은 그 중에서 중요한 visual cues를 선별할 수 있어야 함
    3. 모델은 Long-range 비디오의 계층적 구조를 이해하고 서로 다른 계층 간의 시너지를 활용할 수 있어야 함
- 본 논문에서는 위의 어려움을 기술적으로 해결한 VideoRecap 모델을 제안함
- VideoRecap은 hierarchical video captioning 능력을 극대화하기 위한 세 가지 특징을 가지고 있음
    1. 서로 다른 hierarchical 단계마다 캡션을 생성하기 위한 recursive video-language 아키텍처로 구성됨
    2. 비디오의 계층적 구조를 더 잘 이해하기 위해 짧은 비디오 클립에 대한 학습에서 시작하여 점진적으로 long-range 비디오 클립을 학습하는 curriculum learning 기법을 사용함
    3. Hierarchical하게 captioning된 데이터를 수집하는 것이 매우 어렵기 때문에 LLM을 활용하여 pseudo-summary data, pseudo-annotations를 생성 및 사용함

## 접근법
- VideoRecap은 Video Encoder, Video Language Alignment, Recursive Text Decoder로 구성됨
- Video Encoder로는 기존 모델인 TimeSformer 등을 사용함
- 전체 비디오를 일정한 길이의 클립으로 나누고, 각 클립마다 video features를 추출
- 이때 첫 hiearchy에서는 visual features 자체를 사용하는 반면, 이후 hiearchy에서는 CLS tokens만을 사용함
  - 연산량을 줄이는 효과
  - Low-hierarchy에서는 low-level visual cues를 추출하고, high-hierarchy에서는 higher-level context를 추출함
- Video-Language(VL) alignment 모듈은 video features와 이전 hierarchy에서 생성된 캡션을 입력으로 받아 embeddings를 인코딩함
- VL alignment 모듈의 목적은 video 및 text features를 동일한 공간에 매핑하여 후속 text decoder가 이를 동시에 활용할 수 있도록 하는 것
- Recursive Text Decoder로는 기존 모델의 GPT2 등을 사용함
- Decoder는 VL alignment 모듈에서 인코딩한 embeddings를 입력으로 받아 캡션을 생성함
  - 한 hierarchy에서 생성된 캡션은 다음 hierarchy의 VL alignment 모듈에서 입력으로 사용됨
- 비디오의 hierarchical structure를 더 잘 이해하기 위해 curriculum learning 기법을 사용하여 모델을 훈련시킴
- VideoRecap은 처음에 짧은 비디오 클립에 대한 캡션(clip caption)을 예측하도록 학습되며, 이후에는 segment description, 마지막에는 video summary를 예측하도록 훈련됨
- 하지만, 대규모 비디오에 대해서 hierarchical하게 annotation된 데이터를 구하는 것은 어려움
  - 특히, medium-range의 segment descriptions나 long-range의 video summaries의 경우 더 어려움
- 따라서, 본 논문에서는 LLM을 사용하여 medium-length 및 long-range videos에 대한 pseudo-caption annotations을 생성함 → Ego4D-HCap 데이터셋
  - Clip captions를 입력으로 받아 medium-length segment descriptions와 long-range video summaries를 생성하도록 LLM을 fine-tuning함
## 실험결과
- 직접 만든 hierarchical video captioning 벤치마크인 Ego4D-HCap에서 평가했을 때 SOTA 성능 기록
- Long-range videoQA 데이터셋인 EgoShema에서 평가했을 때도 좋은 성능 기록
  - VideoRecap 모델로 주어진 비디오에 대한 captions를 생성하고, 해당 captions를 GPT3.5에 입력하여 답을 구함

## 의견
- Hierarchical한 접근 방법 참고하면 좋을 듯
- Ego4D-HCap의 medium-length segment descriptions와 long-range video summaries 사용해도 좋을 듯