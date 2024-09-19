# Object Detection via a Multi-Region and Semantic Segmentation-Aware CNN Model (ICCV, 2015)

[논문링크](https://openaccess.thecvf.com/content_iccv_2015/html/Gidaris_Object_Detection_via_ICCV_2015_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/gidaris2015object.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 object representation과 object localization의 두 측면을 개선하여 detection 성능을 향상시키고자 함
- 당시 SOTA detection 프레임워크였던 Overfeat, R-CNN의 성공은 detection task에서 object representation이 매우 중요하다는 것을 보여줌
> - 좋은 representation이란 object의 서로 다른 특징, object의 pure appearance characteristics, object parts의 서로 다른 특징, context appearance, semantics 등을 포착할 수 있어야 함
- 본 논문에서는 이러한 representation을 학습하기 위해 모델의 각 요소가 object의 서로 다른 region에 집중하도록 설계되어 포착된 discriminative한 appearance factors를 다양화할 수 있는 multi-component CNN (multi-region CNN, MR-CNN)을 제안함
- 또한, MR-CNN의 representational power를 강화하기 위해 semantic segmentation과 관련된 요소도 추가로 학습할 수 있도록 함
> - 단, semantic segmentations를 위한 training data 없이
- 또한, object localization은 detection 성능 증가를 방해하는 bottleneck으로 작용함
- 따라서, 본 논문에서는 MR-CNN에 bbox regression을 수행하는 CNN 모델을 결합하여 localization 성능을 보다 개선하고자 함

## 접근법
- MR-CNN은 activation maps module과 region adaptation module로 구성됨
- Activation maps module은 VGG-16 모델을 사용하여 입력 이미지로부터 activation maps를 추출함
- 다음으로 region adaptation module은 입력 이미지의 어떤 regions가 주어졌을 때, 각 region를 activation maps 상에 project/crop하고 features를 추출함
> - 먼저, R-CNN과 마찬가지로 selective search를 적용하여 입력 이미지에서 candidate detection boxes를 찾음
> - 이때 각 detection box에 대해 regions를 생성함
> - Regions는 원본 detection box, 원본 detection box의 상하좌우 50% 영역, 여러 scale로 조정한 box 등 10개의 서로 다른 방식으로 정의됨
> - 이후 각 region은 spatial pyramid pooling layer를 통해 activation maps 상에서 고정된 크기의 feature vector로 추출되어 fully-connected layers를 거침
> - 각 detection box의 모든 regions에 대한 features를 concatenate하여 그것을 해당 detection box의 최종적인 representation으로 사용함
- 이렇게 regions를 생성하고, 각 regions마다 독립적으로 features를 추출하는 것은 제한적인 visible parts를 기반으로 차별화된 object features를 생성하도록 장려함
- 또한, MR-CNN의 representational power를 더 강화하기 위해 semantic segmentation-aware features를 학습하도록 함
- 이를 위해 먼저 activation maps module은 background(bg)/foreground(fg)의 두 경우를 예측하는 FCN으로 변경하고 학습시킴
> - 이때 별도의 semantic segmentation annotations를 사용하지 않고 bbox 좌표만 사용하여, 어떤 이미지에 대해 bbox 내의 pixels는 fg로, 그 외는 bg로 pseudo-labeling함 (weakly-supervised way)
> - FCN인 fg segmentation task에 대해 훈련이 끝나면, 마지막 classification layer를 제거하고 나머지 layers를 semantic segmentation-aware activation maps 추출을 위해 사용함
- 이 경우 activation maps는 이밎 object에 대한 정보를 일부 포함하고 있으므로 region adaptation module은 각 candidate detection box를 1.5배한 하나의 region만을 처리함
- 기본 MR-CNN features와 semantic segmentation-aware CNN features를 concat하여 최종적인 features로 사용함
- 또한, activation maps를 입력으로 받아 candidate detection box를 1.3배한 region에 대해 반복적으로 bbox regression을 수행하는 CNN 기반의 module을 추가로 부착함

## 실험결과
- VOC 데이터셋에서 MR-CNN은 R-CNN보다 좋은 성능을 기록하였으며, semantic segmentation-aware features를 추가로 사용했을 때 성능은 더욱 개선되었음
- 또한, object localization 측면에서도 기존의 방법들보다 뛰어난 성능을 보여주었음

## 의견
- / 