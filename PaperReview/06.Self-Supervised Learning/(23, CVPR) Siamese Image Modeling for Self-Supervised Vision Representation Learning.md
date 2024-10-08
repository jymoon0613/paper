# Siamese Image Modeling for Self-Supervised Vision Representation Learning (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Tao_Siamese_Image_Modeling_for_Self-Supervised_Vision_Representation_Learning_CVPR_2023_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/tao2023siamese.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Vision tasks에서 가장 대표적인 self-supervised learning framework는 instance discrimination(ID)과 masked image modeling(MIM)임
> - ID는 같은 이미지로부터 augmented된 views의 표현을 같아지도록, 다른 이미지로부터 augmented된 views의 표현은 멀어지도록 훈련하는 방식
> - MIM은 masked된 이미지로부터 원래의 content를 reconstruct하도록 훈련하는 방식
- 하지만 두 프레임워크는 각각의 단점이 존재함
> - MIM은 각 이미지에 대해 독립적으로 적용되므로 inter-image relationship을 모델링할 수 없고, 이는 의미론적으로 유사한 images의 표현이 잘 align될 수 없다는 것을 의미함
> - ID는 전체 이미지에 대한 global한 표현만을 사용하므로 intra-image structure를 모델링할 수 없고, 이는 features의 spatial sensitivity가 잘 explore되지 않는다는 것을 의미함
- 이러한 딜레마를 해결하기 위해 연구자들은 semantic alignment와 spatial sensitivity의 key factors를 관찰함
> - Semantic alignment를 고려하기 위해서는 유사한 semantics를 갖는 이미지가 인접한 표현으로 projection되어야 함
> - Spatial sensitivity는 이미지 내의 local structure에 대한 모델링 과정이 필요함
- 이러한 관찰 결과를 바탕으로, 본 논문에서는 masked된 view로부터 같은 이미지에서 augmented된 view의 dense representations를 reconstruct하는 Siamese Image Modeling(SiameseIM)을 제안함
> - Semantic alignment와 spatial sensitivity를 한번에 고려함
> - SiameseIM은 여러 tasks에서 ID 및 MIM의 성능을 능가함

## 접근법
- SiameseIM은 online branch와 target branch로 구성됨
- 같은 이미지로부터 두 개의 augmented views $x_a$, $x_b$가 생성됨
- Online branch는 encoder와 decoder로 구성
- Online branch의 encoder는 $x_a$의 visible patches를 latent representation으로 인코딩하고, decoder는 latent representation과 mask tokens을 combine하여 $x_a$의 각 position에 대응되는 $x_b$의 representation을 예측함
> - MAE의 encoder, decoder 구조와 매우 유사
- Target branch는 momentum encoder만으로 구성되어 있으며 $x_b$를 입력으로 받음
- Augmentation은 일반적은 ID의 augmentation list를 사용
> - 단, online branch의 입력($x_a$)에는 mask augmentation이 추가로 적용됨

## 실험결과
- ViT를 backbone으로 사용
- Classification, detection, segmentation tasks에서 기존의 MIM(i.e., MAE) 및 ID(i.e., MoCo-v3)의 성능을 뛰어넘음

## 의견
- /