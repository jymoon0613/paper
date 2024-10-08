# Segmenter: Transformer for Semantic Segmentation (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Strudel_Segmenter_Transformer_for_Semantic_Segmentation_ICCV_2021_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/strudel2021segmenter.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 ViT를 semantic segmentation task에 적용함 (Segmenter)
- CNN 기반의 모델들과 달리 네트워크의 초기부터 global interactions를 모델링할 수 있음

## 접근법
- Segmenter는 encoder-decoder 구조로 구성되어 있음
- 우선 patch embedding된 token sequence가 ViT encoder를 통과함
- 다음으로 encoding된 tokens는 Transformer 기반의 decoder를 통과하여 segmentation maps를 형성함
> - Decoder는 patch-level encodings로부터 patch-level class scores를 예측함
> - Patch-level class scores는 bilinear interpolation을 통해 pixel-level scores로 upsample됨
- Decoder 구조로는 linear decoder와 mask Transformer 두 가지 구조를 비교하여 사용함
- Linear decoder는 patch-level encodings($\in{R^{{N}\times{D}}}$)를 patch-level class scores($\in{R^{{N}\times{K}}}$)로 linear projection함
> - 이후의 2D reshape 및 bilinear upsample 과정을 거쳐 최종 segmentation map으로 변환됨
- Mask Transformer 기반의 decoder는 $K$개(전체 class의 수)의 learnable class embeddings를 사용함
- Patch encodings($\in{R^{{N}\times{D}}}$)와 각 class embeddings($\in{R^{{K}\times{D}}}$)를 곱하여 class masks($\in{R^{{N}\times{K}}}$)를 계산
> - 이후의 과정은 linear decoder와 같음

## 실험결과
- ADE20K, Pascal Context, Cityscapes에서 검증했을 때 좋은 성능 기록함

## 의견
- / 