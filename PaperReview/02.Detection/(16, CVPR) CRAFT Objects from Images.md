# CRAFT Objects from Images (CVPR, 2016)

[논문링크](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Yang_CRAFT_Objects_From_CVPR_2016_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/yang2016craft.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Two-stage detector의 대표적인 프레임워크인 R-CNN은 좋은 성능을 기록하였지만 여전히 문제가 존재함
> - Region proposals에 불필요한 backgrounds 정보가 다수 포함되어 있음
> - 이는 proposals에 대한 classification을 수행하는 detection network가 suboptimal한 object representations를 학습하는 결과로 이어질 수 있음
- 따라서, 본 논문에서는 기존의 two-stage detection 방식을 보다 세분화하여 이러한 문제를 해결하고자 함 (cascade region proposal-network and fasT-rcnn, CRAFT)
> - Region proposals에 대해 object와 background를 구분하는 CNN 기반의 classifier를 추가로 도입
> - Detection network의 object classification 과정에서는 intra-category variance를 보다 집중적으로 학습하기 위해 binary classifier를 추가함 (cross-entropy는 주로 inter-category variance에 대한 학습을 강조함)
- 이러한 stage 구성의 변화를 통해 proposals의 compactness와 localization을 개선하고, ambiguous한 object categories 간의 false positives를 효과적으로 낮춤

## 접근법
- Faster R-CNN의 RPN을 훈련시키고 분석하여 문제점을 도출함
> - Object categories에 대한 전체적인 recall rate은 준수했지만 recall rate는 각 object category 별로 상이했음
> - 또한, 매우 크거나 작은 object의 경우 잘 포착되지 못했음
> - Object의 모양이 매우 단순하여 background와 잘 구분되지 않거나, objects 간에 복잡하게 얽혀있는 경우도 잘 포착되지 못했음
- RPN의 object proposals를 refine하기 위해 classification network를 RPN 직후에 추가함 (FRCN)
> - 주어진 proposals에 대해 real object instances와 background/badly located proposals를 분류함 (2-class detection network)
> - 사전 훈련된 RPN을 정의하고, RPN이 생성하는 2000개의 proposals에 대해 FRCN을 훈련시킴
> - Inference 시에는 RPN이 생성하는 2000개의 proposals에 대해 FRCN이 필터링을 수행하여 final proposals(300개 이하)를 출력함
- Fast R-CNN, Faster R-CNN의 multi-class cross-entropy loss를 사용한 object classification은 많은 false positives를 유발함
> - Multi-class classifier는 object와 non-object(background)를 분류해야할 뿐만 아니라 서로 다른 object categories까지도 분류해야 함
> - 즉, inter-category variances와 intra-category variances를 동시에 모델링해야 함
> - 하지만, training samples의 대부분은 background class로 구성되어 있으므로 intra-category variance보다 inter-category variance 포착에 집중되게 됨
- 따라서, one-vs-rest 방식의 binary classifier를 two-class crossentropy loss의 형태로 사용함 (R-CNN의 SVM과 같은 역할)
> - Multi-class classifer와 달리 binary classifier는 주어진 proposals를 하나의 특정한 object category의 관점에서만 고려하기 때문에 intra-category variance를 집중적으로 학습하기에 적합함
- 먼저, Refined된 proposals에 대해 일반적인 object classification이 수행됨 (primitive detection)
- 다음으로, $N$(num. of classes)개의 2-class cross-entropy losses로 구성된 FRCN-2가 연결되어 훈련됨
> - FRCN-2에서는 'background' class가 제거됨
> - Inference 시에는, FRCN-2의 output scores와 primitive detection scores를 곱하여 최종 detection scores를 계산함

## 실험결과
- FRCN을 사용하는 경우 proposal evaluation을 수행했을 때 더 적은 proposals로도 향상된 recall rate을 기록할 수 있었음
- 또한, FRCN-2를 사용하는 경우 향상된 object classification 성능을 기록함
- 최종 object detection task에서도 향상된 성능을 기록함

## 의견
- / 