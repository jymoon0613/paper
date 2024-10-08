# SDRV: Real-Time On-Device Subtitles Detection, Recognition and Voicing (IEEE ICASSPW, 2023)

[논문링크](https://ieeexplore.ieee.org/abstract/document/10192952)

<p align="center">
    <img width="700" alt='fig1' src="../img/degtyarenko2023sdrv.png?raw=true"></br>
    <em><font size=2>Overview.</font></em>
</p>

## 연구목적
- 자막을 읽어주는 기능은 자막을 읽을 수 없는 사용자를 위해 필수적인 기능임
- 딥러닝 기반의 text recognition 방법들이 제안되었지만 대부분 단일 이미지 기반의 처리를 하기 때문에 temporal 정보가 중요한 video 환경에서는 부적합하며 computational costs도 매우 큼
- 본 논문에서는 on-device에서 real-time으로 영상 자막을 인식하고 읽어주는 방법을 제안함 (Subtitles detection, recognition, and voicing, SDRV)

## 접근법
- SDRV는 subtitle tracking, text line localization, text binarization, text recognition, TTS의 다섯 단계로 구분됨
- Subtitle tracker는 자막의 존재 여부를 찾고 변화를 추적함
> - 사전 설정한 sampling rate에 따라 연속된 3개의 비디오 프레임을 추출하고 하나의 이미지로 합성함
> - CNN 기반의 classifier는 새로운 자막이 탐지되었는지 여부를 예측함
- MobileNetV2 기반의 SSD로 text line localization을 수행함
> - 자막이 있는 위치(text line)를 bbox로 식별함
- 자막 인식을 위해 복잡한 배경의 영향을 제거하는 text binarization 수행
> - Otsu thresholding 기법 및 U-Net 기반 binarization 적용
- CRNN을 사용하여 자막을 인식함
> - Segmentation 기반으로 text를 구성하는 각각의 문자를 하나씩 인식하는 방법이 아닌, segmentation-free 기반으로 text 영역을 일정한 chunk로 쪼개고, 각 chunk의 image features를 사용하여 전체 text를 인식하는 방법을 사용
> - Segmentation-free 방법이 속도/연산량 측면에서 유리함
- 인식된 text는 TTS 엔진을 통해 음성으로 출력됨

## 실험결과
- U-Net 기반의 binarization을 사용했을 때 다른 방법들보다 높은 인식 성능으로 이어짐
- 실제 영상으로 평가했을 때 만족할만한 인식 성능과 속도를 보임

## 의견
- /