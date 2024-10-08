# DeepBox: Learning Objectness With Convolutional Networks (ICCV, 2015)

[논문링크](https://openaccess.thecvf.com/content_iccv_2015/html/Kuo_DeepBox_Learning_Objectness_ICCV_2015_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/kuo2015deepbox.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Object detection은 기존의 sliding window 방식에서 object proposals를 찾는 방식으로 변화하였음
> - Object proposals는 원본 이미지로부터 object가 있을 것 같은 (= 높은 objectness score를 가지는) regions를 추출하는 bottom-up 방식으로 구현되었음
- 하지만 bottom-up 방식의 region proposals는 object에 대한 high-level semantics를 고려하지 않으므로 suboptimal함
> - Object categories가 공유하는 high-level structures가 objectness에 대한 더 명확한 근거를 제공할 수 있음
- 따라서, 본 논문에서는 보다 semantic하고, data-driven한 objectness 평가 방법에 대해서 다루고자 함
- 구체적으로, CNN 모델은 objects의 가장 핵심적인 low-/mid-/high-level 특징들을 학습하고, 이를 바탕으로 bottom-up 방식으로 생성된 region proposals를 rerank하는 방법을 제안함 (DeepBox)

## 접근법
- DeepBox는 AlexNet을 기본 아키텍처로 사용함
- DeepBox는 두 stages의 훈련 과정을 거침
- 먼저, Deepbox는 object boxes와 background로부터 랜덤하게 샘플링된 boxes를 구분하도록 훈련됨
> - Objects와 background의 차이를 학습함
> - 주어진 이미지에 대해 sliding window 방식으로 5가지 aspect ratios로 구성된 negative boxes를 생성함
> - Ground-truth boxes와의 IoU가 0.5미만인 경우만 negative samples로 사용함
> - 또한, ground-truth boxes에 대해 일정한 x, y 좌표 범위 내에서 약간 variation을 주어 positive samples를 생성함 (perturbed boxes)
> - 마찬가지로, perturbed boxes 중 ground-truth boxes와의 IoU가 0.5이상인 경우만 positive samples로 사용함
- 다음으로, Deepbox는 생성된 bottom-up proposals에 대해서 학습함
- 이전의 학습 stage와 다른 점은 Edge boxes에 기반한 학습을 진행한다는 점임
> - Edge boxes는 이미지에서 edge를 검출하고, 이를 기반으로 object proposals를 생성하는 방법임
-  Sliding window 방식으로 생성된 proposals와 비교했을 때 Edge boxes는 더 많은 edges, texture, object parts 정보를 포함하고 있으며, 따라서 hard examples의 역할을 할 수 있음
> - Sliding window 기반의 학습이 단순히 background와 objects를 구분하도록 훈련하는 것이었다면, Edge boxes 기반의 학습은 object의 전체적인 구조에 대한 정보와 region proposals의 error에 대한 정보를 학습할 수 있음
- 학습 준비/실행 과정을 sliding windows가 Edge boxes proposals로 대체되었다는 점을 제외하면 이전의 학습과 동일함
> - 단, 이번에는 perturbed boxes 중 ground-truth boxes와의 IoU가 0.7이상인 경우만 positive samples로 사용함
> - 또한, ground-truth boxes와의 IoU가 0.3미만인 경우만 negative samples로 사용함
> - IoU가 0.3이상 0.7 미만인 경우는 reject함

## 실험결과
- DeepBox는 Edge boxes에 비해 object proposals의 중요도(ranking)를 더 잘 파악할 수 있었음
> - 생성된 bottom-up proposals에 대해 objectness scores를 계산한 뒤 가장 score가 높은 proposals부터 일정 수(e.g., 1000, 500)의 proposals만 남겼을 때 ground-truth와 매칭되는 proposals가 얼마나 되는지를 평가함
- Proposal rankings를 시각화해본 결과 Edge boxes는 작은 objects가 다수 존재하는 복잡한 scene 이미지에서 proposal rankings가 poor했던 반면, DeepBox는 좋은 proposal rankings를 보여주었음
- Top-100 proposals의 분포를 시각화해본 결과 Edge boxes의 proposals는 이미지 전체에 분산되어 나타나는 반면, DeepBox의 proposals는 objects를 중심으로 dense하게 분포되어 있었음
- 500개의 DeepBox proposals를 사용하는 Fast R-CNN은 500개의 Edge Box proposals를 사용하는 Fast R-CNN, 2000개의 selective search proposals를 사용하는 Fast R-CNN에 비해 성능이 훨씬 좋았음
> - 좋은 object proposals는 detection 성능 향상에 중요하며, 그런 의미에서 DeepBox는 효과적인 method임

## 의견
- / 