# A Small-Sized Object Detection Oriented Multi-Scale Feature Fusion Approach With Application to Defect Detection (IEEE T-IM, 2022)

[논문링크](https://ieeexplore.ieee.org/abstract/document/9720996)

<p align="center">
    <img width="800" alt='fig1' src="../img/zeng2022small.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Surface defect detection과 같은 real-world applications를 위해 이전보다 더 발전된 small object detection framework가 필요함
- 본 논문에서는 small object detection을 위한 atrous spatial pyramid pooling(ASPP) balanced FPN (ABFPN)을 제안함
- 또한, ABFPN을 적용한 improved PCB defect detection (IPDD) 프레임워크를 제안함
> - PCB = printed circuit board

## 접근법
- FPN과 마찬가지로 backbone의 intermediate feature maps를 top-down 방식으로 fusion함
- 이때 마지막 block의 feature maps에 skip-ASPP 모듈을 적용함
> - Skip-ASPP는 모델이 multi-scale의 context information을 추출할 수 있도록 함
> - 서로 다른 dilation의 5개의 dilated convolution을 연속적으로 적용 ($\{3, 6, 12, 18, 24\}$)
> - 이때 각 dilated convolution 진행 시 skip connection을 사용
> - Dilation block의 output과 마지막 block의 feature map을 element-wise sum하여 출력
- Skip-ASPP의 out이 반영된 마지막 block의 feature map으로부터 FPN 적용
- Small tartget detection은 detail한 정보와 semantic한 정보의 균형이 중요하고, 이를 위해 balanced module을 설계함
> - FPN이 적용된 intermediate feature maps에 서로 다른 stride의 max-pooling을 적용하여 같은 크기로 통일시킨 뒤 averaged feature map을 구함
> - Averaged feature map에 nonlocal module 적용하여 global information을 gather함
> - 이후 residual block 하나를 거친 뒤 서로 다른 4개 크기의 output feature maps를 생성
- Cascaded R-CNN으로 detection 수행
- IPDD 프레임워크는 ResNeXt-152 backbone과 ABFPN을 neck으로 사용하고, R-CNN을 detection head로 사용함

## 실험결과
- PCB defect detection task에서 Deformable DETR, FCOS를 포함한 다른 detection methods보다 좋은 성능 기록
- 특히 작은 크기 결함에 대한 탐지 성능이 좋았음

## 의견
- /