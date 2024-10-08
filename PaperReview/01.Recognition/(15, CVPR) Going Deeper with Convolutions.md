# Going Deeper with Convolutions (CVPR, 2015)

[논문 링크](https://www.cv-foundation.org/openaccess/content_cvpr_2015/html/Szegedy_Going_Deeper_With_2015_CVPR_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/szegedy2015going.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝의 발전은 CNN의 발전을 이끌었으며, 이러한 발전은 단지 더 좋은 하드웨어의 성능, 더 큰 dataset, 더 큰 모델 때문이기보다는 새로운 아이디어와 알고리즘, 그리고 개선된 신경망 구조 덕분임
- 본 논문에서는 정확도에 대한 고려와 함께 연산량 부담을 줄여 실세계에 적용 가능한 recognition model을 개발하고자 함
- 따라서, 본 논문에서는 vision tasks를 위한 효율적인 deep neural network 구조인 GoogLeNet을 제안함 (or Inception)
- 이때 deep은 (1) Inception module의 형태로 새로운 차원의 구조 도입, (2) 네트워크의 깊이 증가라는 두 가지 의미를 지님

## 접근법
### (1) Motivation
- 모델의 성능을 높일 수 있는 가장 직관적인 방법은 네트워크의 사이즈를 키우는 것 (depth, width)
- 하지만 단순히 네트워크의 사이즈를 키우는 것은 (1) 제한된 데이터 양에 비해 모델의 파라미터 수가 압도적으로 많아 과적합을 유발하고, (2) 증가된 사이즈에 비례하여 연산량도 증가한다는 단점이 있음
- 이러한 문제를 해결하기 위해 모델을 기존의 fully-connected한 구조(weight들의 대부분이 0이 아닌 값을 갖는 형태)에서, 입력 layer에서 출력 layer로 향하는 layer 간의 관계를 통계적으로 분석한 후 연관 관계가 높은 것들만 연결하여 최적의 sparsely-connected한 구조(weight의 대부분이 0인 형태, dropout을 적용하는 것과 유사)로 변경하는 방법이 있음
- 하지만, 당시의 컴퓨팅 환경은 균일하지 않은 sparse data 구조를 다룰 때 매우 비효율적이며, dense data 구조는 CPU 및 GPU의 사용으로 빠른 연산이 가능해져 빠르게 발전하였음
- 따라서, sparse한 구조들 중 상관도가 높은것들을 clustering하여 유사 dense한 형태를 만드는 방법이 있음
- Inception 구조도 이러한 sparsely-connected한 구조에 대한 실험으로부터 시작됨

### (2) Inception Module
- Inception module의 핵심 아이디어는 CNN을 optimal하게 sparse한 구조로 만들면서, 연산에서는 최대한 dense하게 만드는 것
- Inception module은 1x1, 3x3, 5x5 세 개의 Conv layer와 1개의 max pooling을 사용
- 여러 스케일의 convolution 연산을 활용해 다양한 스케일에서 효율적으로 특징을 뽑아낸 뒤 각각의 결과를 concat해 하나의 output을 생성
- 즉, 피처 추출 과정을 여러 path로 분산시켜 각 path가 sparse해지도록 함 
- 즉, 이전의 CNN처럼 하나의 conv layer가 모든 피처 추출을 하는 것이 아니라, 추출 과정을 3가지 서로 다른 크기의 conv layer로 분산시켜 각 conv layer가 각자의 spatial feature에만 집중할 수 있도록 함 (각자의 spatial features에만 sparse하게 연결)
- 이는 local sparse structure를 찾는 과정으로 볼 수 있으며  네트워크 전체가 sparse해지도록 함
- 반면, 추출된 피처들을 concat하여 dense matrix로 표현함으로써 효율적인 연산을 가능하게 함
- 고차원의 특징들은 output에 가까워질수록 잘 포착되기 때문에 spatial concentration하며, 따라서 3x3와 5x5 conv layer의 비율을 output에 가까워 질 수록 늘려야 함
- 이때, 1x1 convolution을 추가로 사용하여 연산량을 줄여줌
- 하지만, 1x1로 차원의 수를 줄인 경우 feature가 가지고 있는 정보가 압축되면서 원래의 구조보다 성능이 떨어질 수 있음
- 따라서 연산량이 클 때만 1x1 convolution을 추가한 구조를 사용

### (3) GoogLeNet
- GoogLeNet은 Inception module을 쌓아서 만든 네트워크
- 메모리 효율을 고려하여 초반에는 일반적인 conv. layer를 사용하고, 뒤쪽에는 inception module을 사용
- 또한, 3x3와 5x5 conv layer 이전에 1x1 convolution을 사용함으로써 연산량 감소
- 연산효율의 증가는 네트워크의 depth와 width를 증가시킬 수 있게 만들어 줍
- 또한 학습 과정에서 auxiliary classifier를 사용하여 vanishing gradient 문제를 해결

## 실험결과
- 당시 ImageNet에서 SOTA 성능 달성
- Detection에서도 모델 평가

## 의견
- 피처 추출 과정을 여러 conv. layer로 분산시키고(sparse) 결과를 concat하여 처리함으로써(dense) 연산량을 줄이면서 더 깊고 정확한 네트워크 설계