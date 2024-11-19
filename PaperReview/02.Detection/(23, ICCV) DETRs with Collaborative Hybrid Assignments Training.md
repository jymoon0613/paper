# DETRs with Collaborative Hybrid Assignments Training (ICCV, 2023)

[논문링크](https://openaccess.thecvf.com/content/ICCV2023/html/Zong_DETRs_with_Collaborative_Hybrid_Assignments_Training_ICCV_2023_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/zong2022detrs.png?raw=true"></br>
    <em><font size=2>Framework of our Collaborative Hybrid Assignment Training.</font></em>
</p>

## 연구목적
- DETR은 매우 flexible한 detection pipeline이지만, DETR의 one-to-one set matching은 적은 positive queries만을 활용하도록 제한하여 비효율적인 훈련을 야기함
  - Encoder latent features의 discriminability가 (기존 detectors의) one-to-many matching 방식보다 떨어짐
  - Decoder의 비효율적인 attention learning을 유발함
- 본 논문에서는 이러한 문제를 해결하기 위해 collaborative hybrid assignment training scheme을 제안함 (Co-DETR)
- Co-DETR은 auxiliary heads를 추가하여 one-to-many label assignments로 훈련시킴으로써 one-to-one matching의 문제를 보완함

## 접근법
- 먼저, Transformer encoder의 latent features에 대해 multi-scale adapter를 적용하여 feature pyramid를 생성함
  - Multi-scale adapter는 single feature map을 입력으로 받아 연속된 dowm-/up-sampling을 적용하여 feature pyramid를 생성 
- 이후 K개의 decoder head에 대해 auxiliary heads를 추가하여 one-to-many label assignments를 수행함
  - 생성된 feature pyramid는 decoder head와 auxiliary head에 각각 입력됨
  - Auxiliary head는 ATSS 혹은 Faster R-CNN을 사용함
- 각 auxiliary head에 대응하는 decoder head는 auxiliary head에서 positive로 예측된 queries에 대해 정확한 위치와 클래스를 예측함

## 실험결과
- Co-DETR은 encoder latent features의 discriminability를 개선함
- Co-DETR은 Hungarian matching의 불안정성을 줄여 cross-attention learning을 개선함
- Co-DETR은 COCO 및 LVIS에서 SOTA 성능 달성

## 의견
- /