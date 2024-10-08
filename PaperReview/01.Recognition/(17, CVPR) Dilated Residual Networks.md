# Dilated Residual Networks (CVPR, 2017)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Yu_Dilated_Residual_Networks_CVPR_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/yu2017dilated.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN은 입력 이미지로부터 feature map을 추출하며, feature map은 아주 작은 resolution까지 점차적으로 크기가 줄어듦
- 하지만, 이러한 downsampling은 spatial details의 소실을 야기함으로써 성능이 하락할 수 있음
> - 특히, 작은 objects 및 details가 전체적인 context 이해에 중요한 경우 성능 하락이 더 심할 수 있음
- 또한, spatial details의 소실은 classification task에서 pre-trained된 모델이 dense prediction tasks로 transfer될 때에도 적합하지 않을 수 있음
- 따라서, 본 논문에서는 기존 네트워크의 일부 downsampling layer를 dilation으로 대체하여 output resolution을 증가시키고자 함 (spatial details의 소실을 방지)
> - 기본적으로 residual network 구조를 따르며, 논문의 결과로 제안된 dilated residual networks(DRN)은 image classification 성능을 개선함
> - 또한, DRN은 weakly-supervised object localization 및 semantic segmentation 등의 downstream tasks에서도 좋은 성능을 기록함

## 접근법
- ResNet을 기본 구조로 사용함
> - ResNet은 각 stage의 첫 convolutional layer의 stride를 조절하여 downsampling을 진행
- Output resoltuion을 증가시키는 가장 쉬운 방법은 downsampling을 하지 않는 것 (stride 조절 X)
> - 하지만, 이 경우 receptive field가 커지지 않는다는 문제가 있음
> - 매우 작은 receptive field로 인해 context 정보 추출이 거의 불가능함
- 따라서, downsampling을 하지 않으면서도 receptive field를 키우기 위해 dilated convolutions를 사용함
- ResNet의 stage4 및 stage5를 변경함
> - Stage4 및 stage5의 첫 convolution layer의 striding을 제거하여 downsampling을 하지 않음
> - 대신 줄어드는 receptive field 크기를 보상하기 위해 기존 convolution operators를 2-dilated convolutions 및 4-dilated convolutions로 대체함
- 이후의 분류 과정은 기존과 같음
- DRN과 ResNet의 레이어 및 파라미터 수는 같음
> - 단, ResNet이 입력 이미지를 초기 resolution보다 32배 작은 feature map으로 downsampling한다면, DRN은 8배 작은 feature map으로 downsampling함
- DRN은 image classification에서 훈련된 후 별도의 tuning 과정 없이도 바로 object localization 및 segmentation tasks에 적용될 수 있음
> - Output resolution이 크므로 dense pixel-level class activation maps를 바로 출력할 수 있음
- 하지만 dilated convolutions의 사용은 gridding artifacts를 유발할 수 있음
> - Grid 패턴으로 나타나는 노이즈
- 따라서, DRN의 output activation maps에 존재하는 gridding artifacts를 제거하기 위한 방안을 제안함 (구조 변경)
> - 기존 ResNet 및 DRN의 첫 max pooling layer를 제거하고 convlutional layers로 대체
> - 기존 DRN의 stage 5(4-dilated convolutions) 이후 2-dilated convolutions와 1-dilated convolutions를 연속적으로 추가함
> - 또한 추가된 dilated convolutional layers에서는 residual connection을 사용하지 않음

## 실험결과
- DRN은 같은 크기의 ResNet보다 classification 성능이 더 뛰어났음
- Weakly-supervised object localization 및 semantic segmentation tasks에서도 좋은 성능 기록

## 의견
- /