# Where are the blobs: Counting by Localization with Point Supervision (ECCV, 2018)

[논문링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Issam_Hadj_Laradji_Where_are_the_ECCV_2018_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/laradji2018blobs.png?raw=true"></br>
    <em><font size=2>Overall Architecture.</font></em>
</p>

## 연구목적
- Object counting은 traffic monitoring, cell counting 등 많은 vision applications에 사용되는 중요한 task임
- Object counting은 object의 location, shape, size 등을 인식해야 하지만 object detection만큼 정확한 정보가 필요하지는 않음
- 따라서, 본 논문에서는 bbox나 per-pixel annotations가 아닌 오직 point-level annototations만 사용하여 object counting 모델을 학습시키고자 함
> - 다른 annotations에 비해 수집이 매우 간단함
> - Point-level annotations는 object locations에 대한 러프한 정보만 제공하며 object의 size, shape 등의 정보는 제공하지 않음

## 접근법
- FCN 모델을 베이스라인으로 사용함
- FCN 모델의 semantic segmentation loss를 point supervision 기반 object counting 및 localization을 위한 loss로 변경 (localization counting loss, LC loss)
- LC loss는 네 가지 항목으로 구성됨
> - Image/point/split-level loss + false positive loss
- Image-level loss는 모델이 주어진 이미지의 각 pixel에 대해 적어도 하나는 해당 이미지에 포함된 object classes로 분류하도록 함
- Point-level loss는 모델이 point-level annotations가 있는 pixel을 올바르게 분류하도록 함
- Split-level loss는 모델이 N개의 point-level annotations에 대해 하나의 point-level annotation을 포함하는 N개의 blobs를 생성하도록 함
> - Line split 및 watershed split method 사용
- False positive loss는 모델이 point-level annotations를 포함하지 않는 blobs를 예측하지 않도록 함

## 실험결과
- Object counting에서 SOTA 성능 달성

## 의견
- /