# Panda-70M: Captioning 70M Videos with Multiple Cross-Modality Teachers (CVPR, 2024)

[논문링크](https://openaccess.thecvf.com/content/CVPR2024/html/Chen_Panda-70M_Captioning_70M_Videos_with_Multiple_Cross-Modality_Teachers_CVPR_2024_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/chen2024panda.png?raw=true"></br>
    <em><font size=2>Video captioning pipeline.</font></em>
</p>

## 연구목적
- Video-text pair로 구성된 대규모 데이터셋을 수집하는 것은 매우 어려움
- 본 논문에서는 automatic annotation을 사용하여 70M 개의 Video-text(caption) pair을 포함하는 데이터셋을 구성함
  - Description을 포함하여 제목, 자막 등의 다른 text 정보도 같이 사용하여 caption 생성

## 접근법
- HD-VILA-100M 데이터셋의 긴 영상을 내용 일관성에 따라 여러 개의 비디오 클립으로 나눔 
- HD-VILA-100M는 비디오 제목, 자막 등 캡션 생성을 위한 유용한 멀티모달 정보를 포함하고 있음
- 이를 모두 활용하기 위해 5가지 parent 모델을 사용함
  - Video-LLaMA, VideoChat, VideoChat Text, BLIP-2, MiniGPT-4
  - 각 modality에 맞는 captioning 과정을 설계
- 서로 다른 modality의 parent 모델은 captioning을 잘 하는 비디오의 종류도 다를 것임
  - 비디오를 다루는 parent 모델은 temporal information 활용이 중요한 복잡한 비디오에 대한 captioning을 잘함
  - 이미지를 다루는 parent 모델은 대규모 image-text pair로 학습되었기 때문에 rare한 objects를 포함하는 비디오에 대한 captioning을 잘함
  - 시각적으로 이해하기 힘든 비디오에 대해서는 VQA 모델이 captioning을 잘함
- 여러 parent 모델이 생성한 캡션에 대해 Video-to-Text Retrieval 모델을 사용하여 optimal한 캡션을 선택함
- 이러한 과정으로 생성된 캡션과 parent 모델을 사용하여 student captioning model을 학습시킴

## 실험결과
- 생성된 데이터셋으로 학습하는 경우 video captioning, video-text retrieval, text-to-video generation에서 기존 데이터셋 대비 좋은 성능을 기록함

## 의견
- /