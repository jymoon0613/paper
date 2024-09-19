# Hybrid Knowledge Routed Modules for Large-scale Object Detection (NIPS, 2018)

[논문링크](https://proceedings.neurips.cc/paper/2018/hash/72da7fd6d1302c0a159f6436d01e9eb0-Abstract.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/jiang2018hybrid.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 현재까지의 detection system은 object categories가 많고 long-tail로 구성되어 있는 large-scale detection task, heavy occlusion, small-sized object에 대한 성능이 낮다는 문제가 있음
- 본 논문에서는 detection system의 reasoning 과정을 강화하기 위해 explicit/implicit knowledge를 결합하여 전달하는 Hybrid Knowledge Routed Modules(HKRM)를 제안함
> - Explicit knowledge: linguistic knowledge
> - Implicit knowledge: common spatial layouts

## 접근법
- 먼저, RPN을 통해 각 prpopsal region별 features를 추출함
- Explicit knowledge module과 implicit knowledge 모듈은 region features에 대해 각각 adaptive한 region-to-region graph를 생성함
- 각 모듈은 각자가 담당하는 knowledge로 features를 업데이트하고 output함
- 모듈로부터 output된 features는 concat되어 detection branch로 입력됨
- Explicit knowledge module은 attribute knowledge와 pairwise relationship knowledge를 처리함
> - Region features 간의 L1 distance를 바탕으로 region-to-region graph를 생성함
> - Ground-truth class-to-class graph로 supervised하게 학습함
> - Inference 시에는 입력 region features에 대해 생성된 region-to-region graph로 입력 region features를 calibrate함
- Implicit knowledge module은 common spatial layout을 처리함
> - Explicit knowledge module과 마찬가지로 region-to-region graph 생성 방식을 학습
> - 이때 1개가 아닌 m개의 graph를 생성함 (multi-head)
> - m개의 graph를 평균하여 하나의 graph로 만들고 마찬가지로 입력 region features를 calibrate함

## 실험결과
- MS COCO에서 Faster R-CNN보다 좋은 성능 달성

## 의견
- / 