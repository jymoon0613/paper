# Dynamic MLP for Fine-Grained Image Classification by Leveraging Geographical and Temporal Information (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Yang_Dynamic_MLP_for_Fine-Grained_Image_Classification_by_Leveraging_Geographical_and_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2022dynamic.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-grained recongnition 성능을 높이기 위한 한 가지 방법으로 visual 정보 이외의 추가 정보를 사용하여 multi-modal 접근을 사용하는 방법이 있음
- 특히 geographical and temporal information이 분류 성능을 높이기 위한 discriminative한 정보를 제공할 수 있음
- 하지만 각기 다른 정보를 단순히 concatenation하는 것만으로는 큰 효과를 거둘 수 없음
- 추가 정보의 효용을 극대화하기 위해서는 multi-model representations 간의 고차원 interaction이 필요함
- 따라서 본 연구에서는 추가 정보를 가중치로 사용하여 이미지 피처의 표현력을 증진시키는 dynamic MLP 방법을 제안함
- 즉, 이미지의 visual features는 multimodel features에 의해 discriminative해지도록 feature space로 projection되며, 이러한 projection 과정은 image feature와 multimodal feature의 고차원 interaction을 의미하고 생김새가 비슷한 class를 구분하는 데 더 효과적임

## 접근법
- 먼저 visual features는 backbone CNN의 최종 average pooling을 거쳐 추출되며, additional information features는 residual MLP backbone network를 거쳐 추출됨
- 계산량을 줄이기 위해 visual features의 channel을 줄이고, visual features와 additional information features는 N개의 recursive dynamic MLP blocks를 거치게 됨
- 이후 dynamic MLP blocks를 거친 visual features는 다시 channel이 증가되며 original visual features와의 skip-connection을 거침
- 이러한 방법은 highquality visual contextual clues를 추출하도록 할 뿐만 아니라 multimodal features에 기반하여 최적의 image representations을 attention하는 효과가 있음

## 실험결과
- 4가지 fine-grained 벤치마크인 iNaturalist 2017, 2018, 2021와 YFCC100M-GEO100에서 평가함
- 대다수 데이터셋에서 SOTA 성능 달성

## 의견
- /