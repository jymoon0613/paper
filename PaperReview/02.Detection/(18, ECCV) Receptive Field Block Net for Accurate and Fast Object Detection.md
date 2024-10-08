# Receptive Field Block Net for Accurate and Fast Object Detection (ECCV, 2018)

[논문링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Songtao_Liu_Receptive_Field_Block_ECCV_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/liu2018receptive.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- One-stage detector의 성능 향상을 위한 hand-crafted mechanisms 사용을 제안
- 본 논문에서는 가벼운 CNN의 deep features를 강화시키기 위한 Receptive Field Block(RFB)을 제안
> - RFB는 dilated convolution으로 서로 다른 크기의 receptive fields의 representations를 추출함
> - RFB를 SSD에 추가 (RFB Net)

## 접근법
- RFB는 multi-branch 형식으로 서로 다른 receptive fields의 representations를 추출함 (Inception module)
> - 5x5, 3x3, 1x1 filter를 병렬로 구성
- 또한 각 branch에는 서로 다른 dilation rate의 dilated convolution을 연결
- 모든 branch의 output이 concat되어 detection head로 전달
- SSD에 RFB 적용
> - 기존 SSD에서 사용하는 feature maps와 함께 CNN backbone의 일부 feature maps에 RFB를 연결하여 추가로 생성한 feature maps를 detection head로 전달

## 실험결과
- MS COCO, PASCAL VOC 2007에서 SSD, DSSD보다 향상된 성능을 기록
- SSD와의 속도 차이가 크지 않았으며, DSSD보다는 7배가량 빠름

## 의견
- / 