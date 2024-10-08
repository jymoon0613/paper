# Revisiting Self-Supervised Contrastive Learning for Facial Expression Recognition (BMVC, 2022)

[논문링크](https://arxiv.org/abs/2210.03853)

<p align="center">
    <img width="600" alt='fig1' src="../img/shu2022revisiting.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근의 표정인식의 성능 발전은 많은 양의 labeled facial dataset으로부터 기인함
- 하지만, (1) facial 이미지에 label을 다는 데에는 많은 양의 시간, 노력 및 전문 지식이 필요하며, (2) 서로 상이한 수준의 전문 지식으로 인해 label 불일치 및 노이즈가 포함될 수도 있고, (3) 대부분의 FER 데이터셋은 imbalanced하므로 바로 supervised하게 훈련시키면 성능이 제한적일 수 있음
- 따라서, annotated data의 필요성을 최소화할 수 있는 방안이 필요하며, self-supervised learning(SSL)이 그 해결책이 될 수 있음
- 최근 image contrast 기반의 SSL 방법들이 좋은 성능을 보여주지만 이를 FER에 바로 적용하기에는 문제가 있음
> - 표정 이미지는 expression을 포함하여(expression-related information), head poses, identity, makeup, hair style 등(identity-related information) 여러 요소가 복합적으로 존재하는 복잡한 이미지이며, 네트워크가 불필요한 요소들을 배제하고 expression에 대해서만 학습하도록 규제하는 것이 거의 불가능함
- 따라서, 본 논문에서는 현재의 contrastive learning을 revisit하고 FER에 특화된 방법을 설계할 수 있는 3가지 솔루션을 제안함
> - Positive pairs를 위한 더 강력한 augmentation methods 추가
> - Hard negative pairs의 수 증가
> - False negative paris의 수 감소

## 접근법
### (1) Positives with Same Expression
- 짧은 time interval에서 facial expression은 크게 달라지지 않는다는 가정하에 temporal augmentation (TimeAug)를 적용하여 positive pair를 생성함
- 서로 같은 비디오에서 정해진 time interval(약 1초)을 두고 두 프레임을 추출하여 positive pairs로 사용함
- 서로 같은 비디오에서 추출되었으므로 identity-related information에 대한 고민을 덜 수 있고, 모델은 expression-related 정보에만 집중할 수 있음
- 하지만, 같은 expression을 갖는 서로 다른 비디오의 프레임끼리도 강력하게 pull하도록 해줄 필요가 있음
- 이를 위해 가상의 얼굴 이미지를 생성하는 FaceSwap 방식을 사용함
- 이는 얼굴 모양이 facial landmarks에 비해 expression이 아닌 identity를 더 표현하고 있다는 가정에 기반함
- 주어진 이미지($S_{emo}$)에 대해 데이터셋에서 랜덤하게 다른 표정 이미지($S_{id}$)를 선택하고, 두 이미지의 facial landmarks를 detect한 뒤 $S_{id}$의 landmarks를 $S_{emo}$의 것으로 변경함
- 이를 통해 모델은 facial appearance가 아닌 facial landmarks에 기반한 expression에 집중할 수 있음

### (2) Negatives with Same Identity
- Identity-related information으로 인해 유발되는 문제들을 더 완화시키기 위해 HardNeg 방법을 제안함
- 같은 비디오에서 긴 time interval(약 3초)을 두고 추출한 프레임을 hard negative로 사용함
- TimeAug와 마찬가지로 identity-related information에 대한 고민을 최소화하면서 expression-related한 차이만 학습한 수 있음

### (3) False Negative Cancellation
- FER에서 false negatives는 주어진 이미지에 대한 negative samples로 인식되지만 실은 유사한 감정을 나타내고 있는 이미지라고 할 수 있음
- 이러한 false negatives는 semantic한 정보 학습을 방해할 수 있음
- 따라서 본 논문에서는 MaskFN 방법을 사용하여 false negatives의 악영향을 최소화함
- 눈, 입과 같은 landmarks area는 다른 area보다 expression에 대한 직접적인 단서를 포함하고 있는 경우가 많음
- 따라서, 눈과 입의 정보를 false negatives 판별에 사용함
- 먼저 주어진 이미지에서 눈과 입 영역을 detect/crop하고 ImageNet에서 사전 훈련된 backbone을 통해 눈과 입의 피처를 각각 추출함
- 두 피처를 concat하고 미니 배치 내에서 유사도를 계산하여 정렬함
- 한 이미지와 유사도가 가장 높은 상위 N개의 이미지(기존에는 positive도 hard negative도 아닌 이미지들)를 추가적인 positives로 사용함

## 실험결과
- ResNet-50을 백본으로 사용하고, SimCLR의 아키텍처를 사용함 
- VoxCeleb1, AffectNet, FER2013 데이터셋 사용함
- 사전 훈련 후 세 개의 MLP layers로 구성된 predictor를 사용하여 평가 진행 (backbone freeze 및 unfreeze cases)
- 두 개의 donwstream tasks 진행: emotion classification, valence&arousal recognition
- ImageNet에서 사전훈련된 baseline 및 BYOL, MoCov2, SimCLR보다 좋은 성능 기록함

## 의견
- /