# Semantic Image Segmentation with Deep Convolutional Nets and Fully Connected CRFs (ICLR, 2015)

[논문링크](https://arxiv.org/abs/1412.7062)

<p align="center">
    <img width="500" alt='fig1' src="../img/chen2014semantic.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Deep convolutional neural networks (DCNNs)은 여러 vision tasks에서 좋은 성능을 보여주었지만 semantic segmentation에 적용되기에는 몇 가지 문제가 있음
> - 연속된 downsampling으로 인해 dense prediction이 어려움
> - DCNN은 object-centric한 decision을 내리기 위해 spatial transformations에 invariance한 성질을 지녀야 하며, 이는 object의 fine-grained한 특징들을 간과하게 하여 모델의 spatial accuracy를 악화시킴
- 본 논문에서는 이러한 semantic segmentation task에서의 DCNN의 단점을 해결하기 위한 새로운 네트워크 구조를 제안함 (DeepLab)
> - Downsampling으로 인한 문제를 해결하기 위해 atrous convolution(or dilated covolution)을 사용함
> - Object의 fine-grained한 details를 잘 포착하기 위해 fully-connected conditional random field(CRF)를 사용함

## 접근법
- VGG-16 아키텍처에 기반함
- Dense prediction을 위해 VGG-16의 fully-connected layers를 convolution layers로 대체함
> - 입력 크기와 동일한 크기의 output을 만들어야 함
> - Pooling의 hyperparameters(stride, padding, etc.) 조절, atrous convolution 사용, bilinear interpolation을 사용하여 output dimension을 맞춰줌
- 또한, 기존의 DCNN은 이미지 내 object의 대략적인 존재와 위치만을 예측하며 object의 정확한 경계는 잘 잡아내지 못함
> - 이는 classification과 segmentation task의 차이에서 기인함
> - 깊은 모델일수록 많은 subsampling을 통한 invariance의 증가와 넓은 receptive field size 덕분에 classification 성능은 증가하지만 details가 많이 손실되어 object의 정확한 위치를 추정하는 것은 더 어려워짐
- 예로부터 conditional random fields(CRF)가 noisy한 segmentation maps를 smoothing하기 위해 사용되었음 (short-range CRF)
> - CRF는 위치적으로 인접합 pixel들이 서로 같은 label로 할당되도록하는 역할을 함
- 하지만, 본 논문은 CRF를 일부 수정하여 detail한 local structure를 복원하기 위해 사용함 (fully-connected CRF)
> - 기존의 short-range CRF가 local connection 정보만을 사용했다면 fully-connected CRF는 모든 pixel 간의 connection을 고려함
> - 모든 pixel에 대해 비슷한 위치, 비슷한 특징의 픽셀은 같은 label을 할당받도록 하며, 특정 label에 속하는 픽셀 개수를 일정 수준 이상으로 유지하도록 함
- 마지막으로, localization 정확도를 높이기 위해 multi-scale prediction method를 사용함
> - Backbone의 각 stage의 output feature map을 결합하여 최종 output 생성

## 실험결과
- Multi-scale features를 사용하는 것은 성능 향상에 긍정적 효과를 주었음
> - Object의 boundaries를 보다 refine해주는 효과가 있었음
- Fully-connected CRF의 적용은 큰 성능 향상을 가져옴
- PASCAL VOC 2012 test set에서 SOTA 달성

## 의견
- /