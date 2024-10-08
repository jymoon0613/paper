# DeepID-Net: Deformable Deep Convolutional Neural Networks for Object Detection (CVPR, 2015)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2015/html/Ouyang_DeepID-Net_Deformable_Deep_2015_CVPR_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/ouyang2015deepid.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Image classification에서 pre-train된 네트워크를 object detection으로 fine-tuning하는 것은 두 task의 mismatch로 인해 효과가 미묘할 수 있음
> - Image는 이미지에 존재하는 objects의 scale 및 location에 robust하다는 특징이 있음
> - 하지만 detection은 정확한 detection을 위해 objects의 scale 및 location에 대한 정보를 필요로 함
- 따라서, 본 논문에서는 object detection을 위한 새로운 pre-training scheme에 대해 다룸
- 또한, 본 논문은 part deformation handling에서 영감을 받아 공유되는 visual patterns와 그것들의 deformable properties를 학습할 수 있는 deformation-constrained pooling (def-pooling) layer를 제안함
- 결론적으로, 본 논문은 많은 object categories에 대해 feature representation와 part deformation을 동시에 학습하는 object detector인 DeepID-Net을 제안함

## 접근법
- ZF-Net을 backbone으로 사용함
- 먼저 backbone을 ImageNet1K classification에서 pre-train시킨 뒤 ImageNet detection 데이터셋에서 fine-tuning함
- DeepID-Net은 backbone의 마지막 convolution layer 이후 def-pooling을 사용하는 추가 layers를 연결함
> - Feature maps를 part detection maps으로 사용함 ($C$개의 part에 대한 ${H}\times{W}$ 크기의 part detection maps)
> - Def-pooling layer는 입력 part detection maps로부터 deformable penalties를 계산함 (learnable parameters 사용)
> - Part detection maps에 계산된 deformable penalties를 더해주고 max-pooling을 적용하여 출력함
- Hinge-loss를 사용하여 DeepID-Net을 fine-tuning함
- DeepID-Net의 output features에 대해 SVM이 분류를 수행하는데 이때 image classification에서 훈련된 모델의 예측값을 contextual modeling의 목적으로 같이 넣어줌
> - 즉, SVM은 DeepID-Net의 200-class scores와 pre-trained 네트워크의 1000-class scores를 concatenate하여 최종 입력으로 사용함
- 서로 다른 방식으로 정의된 detectors를 다수 정의하고, 모든 detectors의 예측을 종합하여 최종 예측을 수행하는 model averaging 기법을 사용함

## 실험결과
- ImageNet2014 detection task에서 SOTA 달성

## 의견
- / 