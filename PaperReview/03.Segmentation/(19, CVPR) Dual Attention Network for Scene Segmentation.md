# Dual Attention Network for Scene Segmentation (CVPR, 2019)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2019/html/Fu_Dual_Attention_Network_for_Scene_Segmentation_CVPR_2019_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/fu2019dual.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Scene segmentation은 주어진 scene 이미지를 semantic categories(e.g., sky, road, car, bicycle)에 따라 서로 다른 image regions로 segment하는 작업을 의미함
- Scene segmentation task를 잘 수행하기 위해서는 confusing categories(i.e., field and grass) 간의 구분이 원활해야 하며, 서로 다른 appearance로 표현되는 objects(i.e., cars with different scales)를 적절히 인식해야 함
- 이러한 문제를 해결하기 위해 최근에는 multi-scale의 feature maps를 fusion하는 연구가 많이 제안됨
> - 이러한 context fusion 방식은 서로 다른 scale의 objects/stuff를 포착하는 데 도움이 됨
> - 하지만, scene segmentation에서는 objects/stuff 간의 연관성을 global view 차원에서 모델링하는 것이 중요하며, context fusion 방식은 이러한 특징까지는 다루지 못함
- Recurrent neural networks(RNN)가 objects/stuff 간의 global context를 모델링하기 위해 제안되었지만 한계가 존재함
- 따라서, 본 논문에서는 새로운 natural scene image segmentation 프레임워크인 dual attention network(DANet)을 제안함
> - Spatial 및 channel dimensions에서의 feature dependencies를 포착하기 위해 self-attention mechanism 사용
> - 즉, position attention module과 channel attention module을 병렬 구조로 배치

## 접근법
- Dilated conv. 기반의 ResNet을 backbone으로 사용함
- Backbone으로부터 나온 features는 병렬로 구성된 두 개의 attention modules를 통과함
- Local features 간의 rich contextual relationships를 모델링하기 위해 position attention module을 사용함
> - Backbone의 출력 feature maps를 입력받아 convolution layer, reshaping, matrix multiplication, softmax function을 통해 spatial attention map을 생성함
> - 이후 입력 feature map과의 multiplication을 통해 output feature map을 계산 (+ residual connection)
> - Position attention module은 intra-class 간의 응집력을 강화시켜주고 semantic consistency를 향상시켜줌
- Feature maps의 channel은 특정한 semantics에 대한 표현을 지니고 있으므로 channel attention module을 통해 feature channel 간의 상호연관성을 모델링함
> - Backbone의 출력 feature maps를 입력받아 reshaping, matrix multiplication, softmax function을 통해 channel attention map을 생성함
> > - 이후 입력 feature map과의 multiplication을 통해 output feature map을 계산 (+ residual connection)
- 두 attention module을 거친 각각의 output을 element-wise sum하여 fusion함
- Fusion된 feature map을 기반으로 최종 예측 수행

## 실험결과
- Cityscapes, PASCAL VOC2012, PASCAL Context, COCO Stuff에서 평가 진행
- 두 attention modules의 추가는 baseline의 성능을 크게 증가시킴

## 의견
- /