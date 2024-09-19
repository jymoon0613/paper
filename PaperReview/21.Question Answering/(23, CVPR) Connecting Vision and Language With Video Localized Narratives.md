# Connecting Vision and Language With Video Localized Narratives (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Voigtlaender_Connecting_Vision_and_Language_With_Video_Localized_Narratives_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/voigtlaender2023connecting.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Localized Narratives(LN)는 annotators가 말로 이미지를 설명하는 동시에 설명하고 있는 이미지 영역을 마우스 포인터로 가리키는 multimodal annotations를 의미함
> - Voice와 mouse pointer를 synchronization시킴으로써 모든 word에 대한 dense한 시각적 정보를 획득할 수 있음
- 본 논문에서는 이미지에 대한 LN(ImLN)을 비디오로 확장하는 효과적인 방법을 소개함(VidLN)
> - 제안하는 방법을 통해 OVIS, UVO, Oops 데이터셋을 annotate함
- 이를 바탕으로 Video Narrative Grounding(VNG)과 Video Question Answering(VideoQA)을 위한 새로운 벤치마크를 만듦
> - VNG task는 입력 narrative에 있는 각 명사(noun)에 대해 segmentation mask를 생성하여 비디오 프레임 상에서 localize해야 함
> - 이때 narrative에는 동일한 명사가 존재할 수 있고, 각 명사의 의미는 context에 따라 다른 의미를 가질 수 있기 때문에 기존의 open-vocaburaly semantic segmentation보다도 어려운 task임

## 접근법
- ImLN을 비디오로 확장하는 가장 쉬운 방법은 단순하게 비디오가 재생되는 동안 annotator가 마우스를 움직이면서 말하도록 하는 것
> - 하지만, 이는 'race against time'으로 이어져 거의 하나의 object에 대한 설명만 가능하도록 함
- 따라서, 본 논문에서는 annotator가 비디오에 관해 충분히 설명할 수 있도록 키프레임 기반의 annotation protocol을 제안함
- 우선, annotator는 비디오를 완전히 이해할 수 있도록 충분히 비디오를 시청함
- 이후 annotator는 주어진 비디오에서 설명할 대상을 선정
- 비디오 프레임을 uniform하게 샘플링하여 키프레임 candidates을 추출하고, annotator는 선정한 대상을 설명할 키프레임을 다시 선택
- 선택된 키프레임에 대해 LN 수행
> - 모든 word에 대한 정확한 localization을 획득하기 위해, annotator는 하나의 object에서 다른 object로 설명을 전환할 때 말을 잠깐 멈추도록 함
> - spurious traces가 생기는 것을 방지하기 위해 다른 키프레임으로 전환하거나 다른 object 위치로 전환할 때 mouse trace를 잠시 끔
- 마지막으로 annotator는 자신의 설명을 듣고 manual transcription을 작성함

## 실험결과
- VNG task는 raw 비디오와 text description을 입력으로 받음
> - 이때 text description에는 각 명사가 mark되어 있음
> - Method는 mark된 각 명사에 대해 비디오 프레임에서 segmentation mask를 생성함
- VidLN annotations로 VideoQA의 Q+A pairs를 생성함
- VideoQA는 주어진 질문에 대해 text로 답하는 text-output questions와 질문에 대해 spatial location을 반환하는 location-output questions를 포함함

## 의견
- /