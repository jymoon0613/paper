# NetAdapt: Platform-Aware Neural Network Adaptation for Mobile Applications (ECCV, 2018)

[논문링크](https://openaccess.thecvf.com/content_ECCV_2018/html/Tien-Ju_Yang_NetAdapt_Platform-Aware_Neural_ECCV_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2018netadapt.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Deep neural networks(DNN)의 효율성을 증진시키기 위한 최적의 네트워크 구조를 찾고자 하는 노력이 있었음
- 대부분은 MACs나 weights의 수와 같은 간접적인 평가지표를 optimize하고 있음
- 하지만, latency, energy consumption과 같이 실제로 적용되는 real applications에 관한 직접적인 평가지표를 고려하는 것만큼 효과적이진 않음
- 본 논문에서는 platform-specific한 DNN을 활용할 수 있도록 platform-aware한 알고리즘인 NetAdapt를 제안함
> - 사용자는 NetAdapt를 통해 platform의 resource budget을 충족하면서 정확도는 최대가 되는 pre-trained network 구조를 자동으로 찾을 수 있음

## 접근법
- 주어진 resource budget을 찾을 때까지 네트워크 구조를 변경하는 과정을 반복
- 본 논문에서는 latency를 단일 평가지표로 고려
- 네트워크의 각 layers에 대해 (i) 몇 개의 filters를 사용할 것인지 결정하고, (ii) 어떤 filters를 남길 것인지 결정하고, (iii) 만들어진 네트워크의 fine-tuning accuracy를 저장
- 설정을 변경하면서 설정한 latency 기준을 만족하는지 바로 확인하고 조합을 저장

## 실험결과
- MobileNet을 기준으로 NetAdapt를 적용해봄
- 동일 latency 기준 NetAdapt를 사용했을 때 모델 정확성이 더 좋았음

## 의견
- /