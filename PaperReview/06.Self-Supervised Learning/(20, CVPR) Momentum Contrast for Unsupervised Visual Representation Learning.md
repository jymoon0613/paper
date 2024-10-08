# Momentum Contrast for Unsupervised Visual Representation Learning (CVPR, 2020)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2020/html/He_Momentum_Contrast_for_Unsupervised_Visual_Representation_Learning_CVPR_2020_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/he2020momentum.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- NLP에서는 unsupervised한 representation learning이 큰 성공을 거두었지만, CV에서는 여전히 supervised pre-training이 지배적임
- 최근 CV에서 제안되고 있는 contrastive loss를 사용하는 방법들은 일종의 dictionary learning처럼 query 이미지가 주어지면 데이터로부터 샘플링된 key 이미지들과 비교하여 matching key 이미지에 대해서는 유사해지도록 학습하고, 그렇지 않은 경우는 멀어지도록 학습함
- 이러한 일련의 과정은 contrastive loss를 최소화하는 방식으로 달성할 수 있음
- 본 논문은 contrastive loss를 활용한 unsupervised representation learning을 강화하기 Momentum Contrast (MoCo) 방법을 제안함
  
## 접근법
- Contrastive learning의 효과 극대화를 위해서는 퀄리티 높은 negative samples를 위해 dictionary의 크기는 커야하며, dictionary keys를 위한 encoder는 consistent해야함
- 기존의 end-to-end 방식은 하나의 미니 배치에서 augmented views를 생성하고, 같은 이미지에서 augmented된 views를 제외한 나머지를 negative samples로 사용했지만, 이는 매우 제한적인 negative samples를 사용한다는 문제가 있음 (배치 사이즈 증가가 해결방안이 될 수도 있지만 이는 많은 컴퓨팅 자원을 요구)
- 또 다른 방법인 memory bank 방식은 많은 negative samples를 사용하기 위해 memory bank를 사용하여 모든 이미지의 augmented views를 저장하여 사용하지만, 이는 하드웨어 자원이 많이 필요하다는 점과 encoder의 학습에 따라 각 samples에 대한 표현이 변화하는 것을 반영하지 못한다는 문제가 있었음
- 이러한 통찰로부터 MoCo는 dictionary 구조로 queue를 사용함
- 하나의 배치에 대해 생성된 augmented views를 positive samples롤 사용한 후 이를 queue에 저장하여 이후의 negative samples로 사용함
- 이때 정해진 queue의 사이즈가 한계에 도달하면 가장 오래된 key 이미지를 deque함
- 이러한 progressively replaced 방식을 사용하면 많은 negative samples를 사용할 수 있음과 더불어 encoder에 업데이트에 따른 표현의 변화를 효과적으로 반영할 수 있음
- 하지만 negative samples가 많아진만큼 key encoder에 대한 backpropagation이 intractable해짐
- 가장 단순한 해결책으로 query encoder의 weights를 share하는 방법이 있지만, 이는 key encoder를 너무 빠르게 update하여 representation consistency를 악화시키는 결과를 초래함
- 따라서, momentum update 방법을 사용하여 key encoder의 update 속도를 조절함으로써 consistency를 유지함

## 실험결과
- Linear evaluation에서 MoCo는 모든 batch size 설정에서 end-to-end, memory bank 방식보다 성능이 좋았음
- 기존의 unsupervised representation learning 방법들보다 linear evaluation 성능이 가장 높았음
- Transferable한 generalized된 features를 학습하는지 확인하기 위해 object detection에 대한 transfer learning 성능을 평가하였으며, 다른 방법들에 비해 우수한 성능 기록

## 의견
- Contrastive learning의 대표적인 방법
- 실험 매우 다양하게 하고 분석도 자세함