# CornerNet: Detecting Objects as Paired Keypoints (ECCV, 2018)

[논문링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Hei_Law_CornerNet_Detecting_Objects_ECCV_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/law2018cornernet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- One-stage detector는 효율적인 동작을 유지하면서 detection 성능을 높이기 위해 anchor boxes를 활발히 사용함
- 하지만 anchor boxes를 사용하는 것은 다음과 같은 문제를 야기함:
> - (i) Anchor boxes가 이미지에 존재하는 ground-truth boxes를 전부 충분히 overlap하기 위해서는 매우 많은 anchor boxes가 필요하며, ground-truth boxes와 겹치는 anchor boxes를 제외한 나머지 anchor boxes는 negative boxes로 분류되어 훈련 과정에서 positive/negative anchor boxes의 극심한 imbalance를 야기함
> - (ii) Anchor boxes를 사용하면, 얼마나 많은 boxes를 사용하는지, 그 size와 aspect ratio는 어떻게 정의할 것인지와 같은 매우 많은 hyperparameters와 design choices를 고려해야 함
- 본 논문에서는 anchor boxes를 사용하지 않는 새로운 one-stage detector 프레임워크인 CornerNet을 제안함 
> - Object를 bounding box의 top-left corner, bottom-right corner로 구성된 keypoints pair로 예측함
> - Top-left corner, bottom-right corner를 식별하기 위한 corner pooling layer를 도입

## 접근법
- Hourglass network를 backbone으로 사용함
- Hourglass network에 top-left corners, bottom-right corners를 예측하는 두 개의 prediction modules가 각각의 corner pooling layer와 함께 연결되어 있음
- 두 prediction modules는 각각 ${H}\times{W}\times{C}$의 heatmaps를 예측함 ($C$는 object categories의 수)
> - Heatmaps의 각 channel은 해당 class에 속하는 coners의 위치를 indicate하는 binary mask임
- 하나의 class에 해당하는 여러 개의 obejects가 존재할 수 있고, 이러한 경우 알맞은 top-left corner, bottom-right corner pair를 찾기 위해 embedding vector를 이용함
> - 각 coner에 대해 간단한 embedding vector로 project하고, embedding space에서의 거리가 가까운 top-left corner, bottom-right corner pair를 배정함
- Corner pooling은 각 corner location을 더 잘 localize하기 위해 고안됨
> - Top-left corner를 식별하기 위해 feature maps의 모든 pixel 위치에서 left-right max-pooling을 수행한 결과와 bottom-top max-pooling을 수행한 결과를 더하여 pooled feature maps를 생성함
> - Bottom-right corners에 대해서도 유사하게 정의됨
- 하나의 prediction module은 입력 feature maps에 대해 heatmaps, embeddings, offsets를 예측함

## 실험결과
- MS COCO dataset에서 다른 one-stage detectors보다 향상된 성능을 보여줌

## 의견
- / 