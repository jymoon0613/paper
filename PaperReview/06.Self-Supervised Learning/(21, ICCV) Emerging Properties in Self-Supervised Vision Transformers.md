# Emerging Properties in Self-Supervised Vision Transformers (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Caron_Emerging_Properties_in_Self-Supervised_Vision_Transformers_ICCV_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/caron2021emerging.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Transformers는 vision tasks에서 널리 사용되고 있지만 기존 CNN보다 계산 복잡도가 더 높고, 훈련 데이터가 더 많이 필요하며, 추출되는 피처의 unique한 properties가 존재하는 것 같아 보이진 않는다는 문제가 있음
- NLP에서 Transformer가 성공할 수 있었던 이유는 self-supervisde learning이었음
- Vision Transformer(ViT)도 image-level visual information을 감소시키는 object category 수준의 supervised learning이 아니라 image에 자체에 대한 제한되지 않은 richer signal을 학습하는 self-supervised learning에 집중할 필요가 있음
- 따라서, 본 논문에서는 ViT에 self-supervised pre-training을 적용해보고자 하며 supervised ViT에서는 발견되지 않았던 성질들을 밝혀보고자 함
- 본 논문에서 제안하는 프레임워크는 momentum encoder 방식의 knowledge distillation 과정임 (w/o labels, DINO)

## 접근법
- 하나의 이미지로부터 augmented된 views가 student network와 teacher network의 입력으로 사용됨
- 이때 teacher network에는 기존의 (224x224) random crop이 적용된 두 개의 views가 입력됨
- Student network는 local 정보를 학습하기 위해 보다 작은 resolution(96x96)의 random crop이 적용된 6개의 views가 입력됨
- Student network는 cross-entropy loss를 통해 teacher network의 embedding을 모방하도록 훈련됨
- Teacher network는 학습되지 않으며 student network의 exponential moving average임
- 추가로 centering과 sharpening을 통해 collapse를 해결하려고 함
- Centering은 teacher의 output에 teacher outputs의 평균을 빼주는 것: 하나의 차원이 dominate한 것을 방지하는 대신에 출력값이 uniform한 결과를 장려함
- Sharpening은 cross entropy에서 softmax의 temperature를 낮은 값을 사용하는 것: 출력값이 uniform 분포가 아닌 sharp한 분포를 갖게 함
- 두 개념을 동시에 사용하여 collapse 해결

## 실험결과
- ViT 및 CNN을 backbone으로 사용했을 때 모두 SOTA 성능 달성
- Self-supervised하게 훈련된 ViT는 supervised ViT 및 CNN과 달리 마치 segmentation mask처럼 배경에 대한 의존도가 낮아지고 object에 더욱 잘 attention함

## 의견
- /