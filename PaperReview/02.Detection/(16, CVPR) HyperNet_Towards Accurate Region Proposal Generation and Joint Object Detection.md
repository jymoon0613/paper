# HyperNet: Towards Accurate Region Proposal Generation and Joint Object Detection (CVPR, 2016)

[논문링크](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Kong_HyperNet_Towards_Accurate_CVPR_2016_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/kong2016hypernet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Faster R-CNN은 region proposal networks를 제안하여 object proposald와 detection을 unified한 framework로 디자인함
> - 하지만 여전히 small objects에 대해서는 성능이 떨어졌으며 bbox localization 정확도 측면에서도 문제가 있었음
- 이러한 논의로부터 저자들은 object proposal과 detection을 위한 features가 더 informative할 필요가 있으며, feature resolution이 어느정도 reasonable한 크기여야 한다고 주장함
- 따라서, 본 논문에서는 deep, coarse information을 shallow, fine information과 결합하여 더 풍부한 features를 만들어내는 Hyper Feature를 제안함

## 접근법
- 네트워크의 각 layer에 대해, low layers에는 pooling, high layers에는 deconvolution을 적용하여 multiple feature maps를 추출함
- 이후 추출된 feature maps 각각에 convolution을 추가로 적용하고, normalize한 뒤 하나로 concat함 (Hyper Feature)
> - Hyper feature는 multiple levels로부터 추론할 수 있으므로 더 효과적이고, 적당한 크기의 feature resolution을 가질 수 있도록 해줌
- 이후 hyper feature에 간단한 CNN 구조를 추가로 연결하여 region proposal과 detection을 수행함
- 이때 region proposal을 위한 연산 순서를 기존과 다르게 일부 변경하여 속도를 크게 개선함

## 실험결과
- RPN보다 localization 성능이 좋았음
- Fast R-CNN, Faster R-CNN과 비교했을 때 detection 성능이 더 뛰어났음

## 의견
- / 