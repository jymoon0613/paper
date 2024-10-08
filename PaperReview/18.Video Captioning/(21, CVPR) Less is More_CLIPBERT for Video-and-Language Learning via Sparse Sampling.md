# Less is More: CLIPBERT for Video-and-Language Learning via Sparse Sampling (CVPR, 2021)

[논문링크](https://openaccess.thecvf.com/content/CVPR2021/html/Lei_Less_Is_More_ClipBERT_for_Video-and-Language_Learning_via_Sparse_Sampling_CVPR_2021_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/lei2021less.png?raw=true"></br>
    <em><font size=2>Overview of CLIPBERT architecture.</font></em>
</p>

## 연구목적
- Video captioning, video question answering과 같은 video-text tasks를 해결하기 위해, 기존 방법들은 우선 pre-trained vision/language models로부터 video/language features를 추출한 뒤, 두 features를 shared embedding space로 mapping하기 위해 multimodal fusion을 수행함
- 하지만 이러한 방법은 target task/domain과 vision/language models가 pre-trained된 tasks/domains 간의 불일치로 인해 성능이 저하될 수 있다는 문제와, vided/language features가 independent하게 학습되기 때문에 multimodal interaction을 학습하기 어렵다는 문제가 있음
- 따라서, 본 논문에서는 기존 방법과는 달리 video와 language를 end-to-end 방식으로 동시에 학습하는 방법을 제안함 (CLIPBERT)
  - 하나의 frame 혹은 매우 짧은 video clip을 입력으로 하여 훈련을 수행함 (sparse sampling strategy)
  - Video-text tasks가 아닌 image-text tasks로 pre-training된 모델을 video-text tasks에 적용함 (vision encoder로 3D CNN이 아닌 2D CNN 아키텍처 사용)

## 접근법
- Sparse sampling strategy에 기반하여 입력 video를 일정한 길이의 short clips로 분할함
- Clips의 일부를 랜덤하게 sampling한 뒤 vision/language encoders는 각각의 sampled clip에 대한 features를 추출함
- 모든 sampled clips에 대한 features를 aggregate하여 최종 예측을 생성함
- Sparse sampling strategy는 하나의 dense video를 sparse한 짧은 clips로 모델링한다는 점에서 모델의 generalization을 향상시켜주는 일종의 data augmentation 역할을 함
- Vision encoder는 3D CNN 아키텍처가 아닌 2D CNN 아키텍처인 ResNet-50을 사용함
- 각각의 sampled clip에 대해, clip을 구성하는 frames 중 일부를 다시 랜덤하게 샘플링하고, 샘플링된 frames로부터 각각 feature map을 추출한 뒤, temporal fusion layer를 통해 temporal-axis로 aggregation하여 하나의 clip-level feature map을 생성함
- 이후 모든 sampled clip feature maps에 2D position embeddings를 추가하고 flattening을 거쳐 visual tokens가 인코딩됨
- Text encoder는 trainable word embedding layer와 trainable position embeddings를 통해 language tokens를 인코딩함
- 이후 visual/language tokens는 12-layer Transformer encoder를 거치면서 cross-modal fusion이 적용되고, 마지막 레이어의 CLS token을 downstream tasks에 사용함

## 실험결과
- Text-to-video retrieval, video question answering 등의 video-text tasks에서 모두 좋은 성능을 보여줌

## 의견
- /