 # Beyond Appearance: A Semantic Controllable Self-Supervised Learning Framework for Human-Centric Visual Tasks (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Chen_Beyond_Appearance_A_Semantic_Controllable_Self-Supervised_Learning_Framework_for_Human-Centric_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/chen2023beyond.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Human-centric visual analysis는 person identification, attribute recognition, person search, pedestrian detection, human parsing, pose estimation 등을 포함함
- 본 논문에서는 여러 human-centric visual tasks를 위한 semantic controllable self-supervised learning framework (SOLIDER)를 제안함
> - Human images에 대한 prior knowledges를 representation learning 과정에 도입
> - Human-centric visual tasks를 위한 semantic information 및 appearance information을 학습
> - SOLIDER는 downstream tasks마다 semantic information과 appearance information의 비율을 controller를 통해 conditional하게 조절할 수 있음

## 접근법
- DINO를 baseline으로 사용하여 visual appearance를 효과적으로 학습
- DINO의 학습 과정을 일부 수정하여 semantic information에 대한 학습을 강화하고자 함
- 학습된 DINO의 teacher representation을 clustering한 결과 representation은 visual appearance에 따라 몇 개의 parts로 분할되었음 (semantic clustering)
> - Semantic clustering 이전에 background와 foreground에 대한 clustering을 우선 진행
> - Foreground cluster에서 semantic clustering 수행
- Semantic clustering 결과에 따라 semantic labels를 할당
> - Human images에 대한 prior을 활용
> - Human images에서 사람의 몸은 이미 전체를 구성하고 머리는 가장 상단에, 발은 하단에 고정되어 나타난다는 점을 활용
> - 모든 이미지의 clustered parts를 y축 죄표의 순서에 따라 배정함
- 배정된 semantic labels에 대해 DINO student는 token-level semantic classification 수행
- 제안하는 방법을 통해 semantic 및 appearance information을 효과적으로 학습할 수 있지만 적용되는 downstream task의 종류에 따라 두 information의 중요도가 다를 수 있음
- 네트워크의 입력으로 두 정보의 반영 비율을 의미하는 $\gamma$를 추가로 입력함
> - Pre-train 이후 $\gamma$에 따라 DINO로 출력된 appearance feature map과 semantic classification 과정에서 출력된 semantic feaature map이 적절히 조합되어 최종 feature map을 생성함
> - Pre-train 시에는 매 iteration마다 $\gamma$를 랜덤하게 할당

## 실험결과
- 6개의 human-centric tasks에서 SOTA와 근접하거나 더 좋은 성능을 보여줌
> - Person re-identification, attribute recognition, person search, pedestrian detection, human parsing, pose estimation

## 의견
- /