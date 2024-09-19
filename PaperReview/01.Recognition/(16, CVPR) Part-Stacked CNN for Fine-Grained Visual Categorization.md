# Part-Stacked CNN for Fine-Grained Visual Categorization (CVPR, 2016)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Huang_Part-Stacked_CNN_for_CVPR_2016_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/huang2016part.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-grained visual recognition 성능은 점차 개선되고 있지만 각 fine-grained 카테고리가 어떻게 구분되는지 human-understandable한 설명력은 아직 부족함
- Fine-grained objects 간의 subtle difference는 대부분 object의 local parts에서 찾을 수 있으므로, 설명가능한 분류 기준을 생성하기 위해서는 이러한 local parts 단위 비교에서 분류 근거를 찾아야 함
- 따라서, 본 논문에서는 fine-grained classification과 함께 object parts에 기반한 해석가능한 classification instructions를 생성하는 Part-Stacked CNN(PS-CNN)을 제안함

## 접근법
- PS-CNN은 localization network와 classification network의 두 부분으로 나뉘어짐
- Localization network는 object parts의 location을 detect함
> - FCN을 사용하여 object parts의 location이 encode된 dense output feautre map을 생성함 (part annotations 사용한 supervised training)
- Classification network는 part stream과 object stream으로 구분됨
> - 모든 object parts에 대해 독립적은 CNN을 훈련시키는 것은 비효율적이므로, 제안하는 part stream에서는 모든 detected object parts가 일정 수준의 conv layer를 공유함 (the first five convolutional layers are shared)
> - 또한, 연산량을 줄이기 위해 추정된 object parts의 중심 위치를 기준으로 일정 크기의 region을 feature map 상에서 crop하여 사용함
- Object stream은 image-level representation을 추출
- Object, parts features들을 모두 concat하여 최종 classification 진행

## 실험결과
- 당시 완전한 SOTA는 아니었지만 efficiency 측면에서 개선점이 있음
- 또한, 학습된 모델을 바탕으로 해석가능한 분류 매뉴얼을 만들 수 있음 (주요 contribution)

## 의견
- /