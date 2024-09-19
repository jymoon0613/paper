# SwinBERT: End-to-End Transformers With Sparse Attention for Video Captioning (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Lin_SwinBERT_End-to-End_Transformers_With_Sparse_Attention_for_Video_Captioning_CVPR_2022_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/lin2022swinbert.png?raw=true"></br>
    <em><font size=2>Overview of the proposed framework.</font></em>
</p>

## 연구목적
- 본 논문에서는 raw video frames를 입력으로 받아 end-to-end로 caption을 생성하는 Transformer 기반 모델을 제안함 (SwinBERT)
- SwinBERT는 임의의 길이의 video frame sequences를 처리할 수 있음
- Densely sampled frames를 입력으로 사용하되, 연속된 frames로 인해 발생하는 redundancy를 해소하기 위해 learnable sparse attention mask를 제안함
  - Sparse attention mask는 모델이 전체 video frame patches 중에서 중요한 spatial-temporal movements(e.g., main moving objects)를 포함하는 patches에 더 집중하도록 하고, 덜 중요하면서 변화가 적은 부분(e.g., background)을 포함하는 patches는 무시하도록 함
  - Sparse attention mask는 learnable하기 때문에 task-specific하게 adaptation할 수 있음

## 접근법
- SwinBERT는 Video Swin Transformer(VidSwin)와 Multimodal Transformer Encoder로 구성됨
- Pre-trained된 VidSwin은 visual encoder로서 raw video frames를 video feature tokens로 인코딩함
- 인코딩된 video tokens는 multimodal Transformer encoder로 입력되고 sequence-to-sequence 방식으로 caption이 생성됨
- Multimodal Transformer encoder에 sparse attention mask를 적용하여 densely-sampled frames에 존재하는 redundancy를 해소함
  - Loss function에 attention mask에 대한 sparsity constraint를 추가하여 핵심적인 visual token relationships만 고려되도록 함
  - Attention mask는 sigmoid function을 사용하여 0-1 사이의 값을 가지는 soft mask임 (0.5를 threshold로 하여 hard하게 변경도 가능)

## 실험결과
- SwinBERT는 video captioning task에서 SOTA 성능 달성

## 의견
- /