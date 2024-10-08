# Your “Flamingo” is My “Bird”: Fine-Grained, or Not (CVPR, 2021)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2021/html/Chang_Your_Flamingo_is_My_Bird_Fine-Grained_or_Not_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/chang2021your.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-Grained Visual Classification (FGVC)은 기계가 인간의 fine-grained한 수준의 object recognition 능력을 따라잡을 수 있는지에 대한 새로운 물음에 기반하여 주목받아 왔음
- 연구의 목적은 주로 fine-grained discriminative features에 집중하여 분류 성능을 개선하는 것
- 하지만 fine-grained class label은 인간도 전문가가 아닌 이상 분류하기 어려움 (새에 대한 전문가가 아니면 어떤 새의 종류가 주어지든 새라는 것만 인식함)
- 따라서, 본 연구에서는 단순히 FGVC의 성능 개선을 넘어 실제로 fine-grained label이 어떻게 인식되는지와 이를 바탕으로 한 실용적인 접근법에 대해 연구함
- 인간 대상의 설문 조사를 통해 사람들은 주어진 fine-grained 분류 문제에 대해 single label을 바로 예측하는 것보다 큰 분류로부터 (새, 고양이) fine-grained class 분류에 이르기까지 (새 종류) 계층적인 multi label 문제로 접근하는 것을 선호한다는 것을 발견함
- 이에 기반하여 본 논문에서는 FGVC 문제를 single label 문제에서 계층적인 multi label 문제로 재정의함
- 즉, 모델은 multi-task learning과 유사하게 계층적인 여러 label을 예측함
- 단순히 fine-grained labels가 주는 정보에 비해 ('flamingo') 계층적인 coarse-to-fine series of labels가 주는 정보 ('bird', 'Phoenicopteriformes', 'Phoenicopteridae', 'flamingo')가 훨씬 풍부함
- 또한 이러한 방식은 별도의 파라미터 증가 없이 기존의 FGVC 모델에 적용될 수 있음

## 접근법
- Multi-task learning과 유사한 흐름을 가짐
- 모든 입력 이미지에 대해 wikipedia에서 추출한 정보를 바탕으로 K개의 클래스 레이블이 지정됨 (계층적 구조)
- CNN 백본으로부터 이미지의 피처를 추출하고, 모든 K개의 레이블을 예측함
- Loss는 CE를 사용
- 추가적인 실험으로부터 (1) coase-label은 fine-grained classifier의 성능을 악화시키고, (2) fine-grained label은 coarse classifier의 성능을 강화시킨다는 것을 발견함
- 따라서, coarse label prediction 결과가 fine-grained classifier의 결정에 영향을 주지 못하도록 별도의 path를 설계하고, coarse label classifier의 결정 과정에 fine-grained classifier가 참여하도록 함
- 백본의 output feature는 K개로 동일한 크기 피처로 나누어짐
- 첫 번째 피처는 가장 coarse한 label prediction에 이용되며 마지막 K번째 피처는 가장 fine-grained한 label 예측에 사용됨 (계층적 구조)
- 이때 k번째 피처에 대한 예측은 이후의 K까지의 피처와 함께 concat된 피처를 입력으로 사용하여 이루어짐
- 즉, 맨 처음의 coase-label 예측에는 모든 K개의 피처가 사용되며, 마지막 fine-grained label 예측에는 K번째 피처만 사용됨
- 이때 fine-grained에 속하는 피처가 coase-label 예측으로 인해 편향되는 것을 방지하고자, 하위 label 예측으로 사용될 때는 gradient를 반영하지 않음

## 실험결과
- CUB-200-2011, Stanford Cars, Aircrafts 데이터셋에서 실험
- 제안하는 방법은 다른 FGVC 벤치마크 모델들에 잘 적용되었으며, 해당 모델들의 기존 성능을 개선함
- 또한, 제안하는 방법을 적용했을 떄 기존 모델의 특성을 헤치지 않는 것으로 보임
- Grad-CAM으로 확인해보았을 때 실제로 모델은 계층적인 구조에 따라 fine-grained label을 예측할수록 이미지의 더 세밀한 부분에 집중하고 있었음
- SOTA 성능도 개선

## 의견
- 아이디어가 참신함
- 설문조사를 통한 결과를 제시하는 등 서론이 매우 탄탄함
- 매우 간단해 보일 수 있는 주제에 대해 human study 결과 등을 제시함으로써 이론적 배경을 탄탄하게 하고 논문의 설득력 및 연구의 퀄리티를 높임
- 방법에 대한 치밀한 검증이 매우 중요함