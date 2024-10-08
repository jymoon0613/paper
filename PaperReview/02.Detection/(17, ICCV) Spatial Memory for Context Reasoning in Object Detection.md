# Spatial Memory for Context Reasoning in Object Detection (ICCV, 2017)

[논문링크](https://openaccess.thecvf.com/content_iccv_2017/html/Chen_Spatial_Memory_for_ICCV_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/chen2017spatial.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Contexts는 image understanding에 효과적이며 이는 object detection에서도 중요함
> - 만약 테니스 라켓이 이미지 내에 존재하는 것을 알고 있다면 높은 확률로 테니스 공도 존재할 것이라 예상할 수 있음
- Contexts를 모델링하는 방법으로는 전체적인 image/scene level context를 모델링하거나 instance간의(instance-level) context를 모델링하는 방법이 있음
- 이 중 instance-level context를 모델링하는 것이 훨씬 어려움
> - Detection tasks에서 instance-level context를 모델링하기 위해서는 서로 다른 classes, locations, scales, aspect ratios를 갖는 bbox pairs를 처리해야 함
> - 또한, 주어진 이미지에서 어떤 objects들이 detect되었는지 기억하고, 이를 또다른 object 예측에 활용해야 하지만, 현재의 detection systems는 모든 detection을 병렬로 처리하므로 이러한 구현이 어려움
- 본 논문에서는 instance-level context를 효과적으로 모델링하기 위한 spatial memory network(SMN)을 제안함

## 접근법
- VGG-16을 backbone으로 사용하는 Faster R-CNN을 baseline으로 사용함
- 2D empty feature maps를 memory로 사용하며, detect된 object의 feature representations(RoI pooling)를 memory에 저장함
- 이후 업데이트된 memory maps는 다음 RPN에서 backbone feature maps와 함께 사용됨
> - Memory maps에 몇 개의 convolutional 연산을 연결함
- 새로운 object가 detect되면 다시 해당 object의 feature representations로 memory maps를 업데이트함
> - 이때 GRU를 사용하여 업데이트함

## 실험결과
- SMN을 사용했을 때 baseline보다 유의미한 성능 향상이 발견됨

## 의견
- / 