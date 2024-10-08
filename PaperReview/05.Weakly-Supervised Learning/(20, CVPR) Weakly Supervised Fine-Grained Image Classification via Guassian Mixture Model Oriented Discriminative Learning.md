# Weakly Supervised Fine-Grained Image Classification via Guassian Mixture Model Oriented Discriminative Learning (CVPR, 2020)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2020/html/Wang_Weakly_Supervised_Fine-Grained_Image_Classification_via_Guassian_Mixture_Model_Oriented_CVPR_2020_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/wang2020weakly.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Weakly Supervised Fine-grained Image Recognition (WFGIR)은 이미지 수준 레이블만을 이용하여 fine-grained한 subtle difference를 찾아내는 것을 목저으로 함
- 이러한 방법은 1) FGVC는 클래스 간 차이를 식별하기 매우 어렵다는 점과 2) part annotations 없이 오직 이미지 수준 레이블만을 사용해야 한다는 점에서 어려움
- 이러한 문제 해결을 위해 patch 단위의 heuristic한 접근방식을 사용하거나 loss function 혹은 correlation guided discriminative learning을 사용한 간접적인 접근방식을 사용하기도 함
- 이러한 접근방식은 high-level 피처맵에서 직접 discriminative regions를 찾으려 한다는 점에서 공통적이지만, high-level 피처맵이 CNN의 local receptive field 내의 공간 및 채널 정보를 융합하여 만들어진다는 점을 간과함 (즉, 포착되는/집중되는 regions의 범위가 큼)
- 이는 결국 high-level 피처맵은 discriminative regions 및 less-discriminative regions 간의 구분이 모호해진다는 것을 의미함 (배경 혹은 노이즈가 포함됨)
- 따라서 NLP의 low-rank mechanism에서 착안한 Discriminative Feature-oriented Gaussian Mixture Model (DF-GMM) discriminative regions가 섞이는 현상을 방지하고 WFGIR 성능을 개선하고자 함

## 접근법
- 제안하는 DF-GMM은 low-rank representation mechanism (LRM)과 low-rank representation reorganization mechanisum (lr2m)으로 구성됨
- 우선 백본으로부터 최종 피처맵을 출력
- Low-rank representation mechanism은 GMM을 통해 high-level 피처맵으로부터 low-rank discriminative bases를 도출하도록 하는 역할을 함
- GMM의 반복적인 update를 거쳐 high-level 피처맵의 diffusion 현상을 개선하여 activation map이 훨씬 discriminative 해지도록 함
- Low-rank representation reorganization은 추출한 low-rank discriminative bases로 부터 피처맵을 재구성함
- 재구성한 피처맵을 Feature Pyramid Network에 투입하여 region proposals (patches)를 추출함
- Classification loss 외에도 hinge loss의 개념을 사용하여 네트워크가 보다 discriminative한 regions를 포착하도록 하고, 찾은 discriminative patches를 combined한 경우 각각의 개별 discriminative 패치를 이용할 때보다 잘 예측하도록 함

## 실험결과
- CUB-200-2011, Stanford Cars, Aircraft에서 평가함
- SOTA 성능 개선
- 시각화를 통해 효과 보여줌

## 의견
- CNN이 receptive field의 중첩으로 인해 disrciminative regions이 거의 global object 수준으로 확산되어 fine-grained한 피처 추출이 어려워진다는 점은 이후의 논문에서 근거로 사용할 수 있음
- 수식 전개하는 방법 참고