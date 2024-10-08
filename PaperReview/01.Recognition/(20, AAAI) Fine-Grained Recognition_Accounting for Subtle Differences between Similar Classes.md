# Fine-Grained Recognition: Accounting for Subtle Differences between Similar Classes (AAAI, 2020)

[논문 링크](https://ojs.aaai.org/index.php/AAAI/article/view/6882)

<p align="center">
    <img width="600" alt='fig1' src="../img/sun2020fine.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Visual Categorization (FGVC)는 클래스 내의 variations에 invariant하면서 클래스 간의 미세한 차이를 포착해야 하기 때문에 어려운 과제임
- FGVC에서 일반적으로 사용되는 input-label mapping 방식의 학습은 효과가 있지만 결정적인 일부의 특징들만을 포착한다는 한계가 존재
- Attention 기반의 모델을 분석해본 결과 attention을 사용하는 경우 아주 적은 수의 local parts만을 포착하는 경향이 있었음
- 따라서, 본 연구에서는 다양한 관련 특징들을 포착하고 이를 통합하기 위해 attention을 분산하는 방법을 제안함 (Diversification Block을 통해)
- 또한, cross-entropy와 같은 loss는 모든 클래스에 대해 계산되지만, 본 연구에서는 일부 강력한 negative classes에만 집중하고 loss를 계산하여 모델의 분별력을 높이고자 함 (Gradient-Boosting Loss를 통해)

## 접근법
- 제안하는 방법은 기존 Deep CNN 모델들에 쉽게 적용될 수 있음 
- 먼저 백본으로부터 output 피처맵이 추출되고, 피처맵은 1x1 convolution을 거쳐 각 클래스마다의 activation map으로 투영됨
- 생성된 클래스별 activation map은 diversicifation block의 입력이 됨
- 먼저 클래스별 activation map과 같은 크기의 mask가 만들어짐 
- 클래스별 activation map의 최대값을 기준으로 필터링하여 peak map을 생성
- Peak map을 기준으로 나머지 불필요 영역을 가리는 마스크를 생성
- Peak mask는 주어진 activation map에서 모델이 가장 중요하다고 생각하는 부분만 남기도록 하는 역할
- Patch suppression은 모델이 다른 local parts에도 attention을 분산시키도록 peak parts를 패치 단위로 일부 마스킹하는 역할을 함
- 최종 suppression mask는 peak mask와 patch mask의 합
- Suppression mask를 기준으로 클래스 별로 activation map을 refine하고 GAP를 마지막으로 수행하여 score를 계산 
- 모델의 분별력을 증진시키기 위해 모든 클래스에 대해 loss를 계산하는 것이 아니라 일부 강력한 negative classes에 대해서만 loss를 계산하는 gradient-boosting cross entropy (GCE) loss를 사용
- 주어진 클래스의 score를 기준으로 score가 높은 상위 k개의 negative classes 정의
- 오직 k개의 negative classes에 대해서만 cross-entropy loss 계산
- GCE가 CE보다 수렴이 빠르다는 것을 간단한 수식으로 증명

## 실험결과
- CUB-200-2011, Stanford Cars, Aircrafts, Stanford Dogs 데이터셋에서 검증
- SOTA 성능 개선

## 의견
- 최종 layer에서 1x1 convolution해서 activation map으로 사용한 것 참신함
- GCE loss는 다른 task에라도 적용해보면 좋을듯