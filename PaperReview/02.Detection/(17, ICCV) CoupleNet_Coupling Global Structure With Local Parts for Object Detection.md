# CoupleNet: Coupling Global Structure With Local Parts for Object Detection (ICCV, 2017)

[논문링크](https://openaccess.thecvf.com/content_iccv_2017/html/Zhu_CoupleNet_Coupling_Global_ICCV_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhu2017couplenet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 global/local 정보 fusion을 통해 object detection 성능을 개선하고자 함 (CoupleNet)
- RPN에서 생성된 object proposals는 두 개의 branches로 입력됨
> - 한 branch에서는 local part 정보를 포착하기 위한 position-sensitive RoI(PSRoI) pooling을 적용함 (R-FCN)
> - 다른 branch에서는 global context 정보를 포착하기 위한 RoI pooling을 적용함 (Faster R-CNN)

## 접근법
- 먼저, RPN으로부터 생성된 region proposals를 fully convolutional한 local branch로 입력됨 (local FCN)
- Local FCN에서는 R-FCN에서 제안된 PSRoI pooling을 적용하여 object-specific parts 정보를 추출함
> - Feature maps에 convolution을 적용하여 ${k}^2(C+1)$의 channels를 갖는 position-sensitive score maps를 얻음
> - 이는 ${k}\times{k}$로 나뉘어진 spatial 그리드의 각 position에서 $C+1$개의 classes (num. object classes + bg)에 대한 score를 평가한다는 의미임
> - 이후 각 object proposals에 대해 position-sensitive RoI pooling을 적용함
> - 마찬가지로, RoI를 ${k}\times{k}$의 bin으로 나누고 position-sensitive score maps 상에서 RoI pooling을 수행함
> - 이때 ${k}\times{k}$의 각 bin은 주어진 region 내에서 position-sensitive score maps의 ${k}\times{k}$의 각 grid에 대해 average pooling을 적용한 것임
> - Position-sensitive RoI pooling을 통해 ${k}\times{k}\times{(C+1)}$의 feature를 추출
> - 마지막으로 ${k}\times{k}$의 bin에 대해 다시 average pooling을 적용하여 $(C+1)$ 차원의 feature vector를 추출함
- 동시에 RPN으로부터 생성된 region proposals는 global FCN으로 입력됨
- Global FCN은 각 proposals에 대해 RoI pooling을 적용하여 features를 추출함
> - 이때 global 정보를 더 효과적으로 추출하기 위해 original object proposal과, 해당 proposal을 2배 키운 proposal 모두에 RoI pooling을 적용함
> - 두 features를 concat한 후 추가적인 convolutions를 적용하여 $(C+1)$ 차원의 feature vector로 인코딩함
- 마지막으로 local 및 global features에 1x1 convolution을 적용하고 featelement-wise sum을 적용하여 features를 결합함

## 실험결과
- PASCAL VOC, COCO에서 Faster R-CNN, R-FCN보다 향상된 성능을 기록함

## 의견
- / 