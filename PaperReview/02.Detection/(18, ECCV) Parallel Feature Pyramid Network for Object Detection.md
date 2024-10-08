# Parallel Feature Pyramid Network for Object Detection (ECCV, 2018)

[논문링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Seung-Wook_Kim_Parallel_Feature_Pyramid_ECCV_2018_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/kim2018parallel.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 네트워크의 depth가 아닌 width를 확장하여 feature pyramids(FP)를 구현하는 parallel FP network(PFPNet)을 제안함
> - 이전의 FP는 네트워크의 여러 layers에서 추출된 feature maps에 기반하여 생성되었다면, PFPNet은 하나의 feature maps에서 FP를 구현함

## 접근법
- VGGNet-16을 backbone으로 사용함
- Backbone의 output feature maps에 SPP를 적용하여 서로 다른 scale의 feature maps를 생성함
- 생성된 feature maps에 bottleneck layers를 적용하여 channel과 resoultion을 일부 압축함
- 마지막으로 multi-scale features를 fusion하기 위해 multi-scale context aggregation(MSCA) modules를 적용함
> - 모든 feature maps를 upsampling/downsampling하여 concat하고, convolutional layers를 몇개 거쳐 다시 기존 resolution의 feature maps로 변환시킴
> - 변환된 feature maps에는 multi-scale의 context 정보가 담겨 있음
- 변환된 feature maps 각각에서 prediction을 수행함

## 실험결과
- PASCAL VOC 2007/2012에서 향상된 detection 성능을 기록하였으며, COCO에서는 RetinaNet과 거의 비슷한 성능을 기록함
> - 단 PFPNet은 RetinaNet, SSD, DSSD보다 빠른 inference speed를 기록함

## 의견
- / 