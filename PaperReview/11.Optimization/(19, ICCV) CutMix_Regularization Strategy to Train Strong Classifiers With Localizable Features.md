# CutMix: Regularization Strategy to Train Strong Classifiers With Localizable Features (ICCV, 2019)

[논문링크](https://openaccess.thecvf.com/content_ICCV_2019/html/Yun_CutMix_Regularization_Strategy_to_Train_Strong_Classifiers_With_Localizable_Features_ICCV_2019_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/yun2019cutmix.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN의 overfitting을 방지하기 위해 hidden activatons의 일부를 랜덤하게 drop하는 dropout이나, 입력 이미지의 일부를 erasing하는 regional dropout과 같은 방법들이 제안되었음
- 특히 regional dropout은 네트워크가 전체 이미지 context를 보도록 함으로써 성능 향상을 이끌었음
- 하지만, 기존의 regional dropout은 erasing된 region을 0으로 채우거나 noise로 대체하였는데 이는 훈련 이미지의 informative한 pixels의 일부가 심각하게 손실된다는 것을 의미함
- 따라서, 본 논문에서는 단순히 pixels을 removing하는 것이 아니라 다른 이미지의 regional patch로 대체하는 augmentation 방법인 CutMix를 제안함
- 이러한 방식은 regional dropout의 이점을 그대로 누리면서 더이상 uninformative한 정보들을 고려할 필요가 없게 해줌 

## 접근법
- 두 이미지와 레이블을 샘플링하고, 두 이미지의 랜덤 bounding box region을 서로 swap하여 새로운 이미지 생성
- 레이블은 mixup과 마찬가지로 섞어서 새로운 레이블 생성
- 실제 구현에서 두 샘플은 같은 미니 배치에서 랜덤하게 샘플링됨
- CutMix는 cutout과 마찬가지로 네트워크가 전체 이미지 context를 보도록 함과 동시에, mix된 두 objects가 단일 이미지의 partial views에서 인식되도록 함으로써 효율성을 높임
> - 실제로 모델의 activation map을 시각화했을 때, 모델은 cutmix된 이미지에서 각각의 object를 별도록 인식하고 있었음
- 또한, CutMix는 overfitting을 방지하고 훈련을 안정화시켜줌

## 실험결과
- CutMix는 cutout 및 mixup과 마찬가지로 image classification 성능을 향상
- 추가로 CutMix는 cutout 및 mixup과 달리 object detection 및 localization에서의 성능 향상도 달성함
- CutMix는 adversarial examples에 대해 mixup 및 cutout보다 강건함
- Occluded samples 및 in-between class samples에 대해서도 강건함

## 의견
- /