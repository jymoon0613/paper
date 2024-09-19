# Vision Transformers for Dense Prediction (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Ranftl_Vision_Transformers_for_Dense_Prediction_ICCV_2021_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/ranftl2021vision.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Dense prediction을 위한 모델 아키텍처는 대부분 CNN에 기반하고 있음
- 하지만, CNN은 multi-scale features를 추출하기 위해 입력 이미지를 지속적으로 downsample하며, 이는 feature resolution과 fine granularity가 중요한 dense prediction tasks에서 문제가 됨 
- 따라서, 본 논문에서는 dense prediction tasks를 위한 Transformer 구조를 제안함 (DPT)
> - DPT는 encoder-decoder 구조로 설계되며, 이때 encoder 구조로 ViT를 사용함
- CNN과 달리 ViT는 초기의 patch embedding 이후로부터는 일정한 크기의 feature resolution을 유지하며, self-attention을 통해 global receptive field를 지니고 있고, 이러한 ViT의 특징은 dense prediction tasks에서 강점이 될 수 있음

## 접근법
- 입력 이미지는 먼저 ViT encoder를 통과함
- 이후 3개의 stage로 구성된 reassemble 연산을 통해 ViT의 intermediate output tokens로부터 image-like representations를 recover함
> - Read: 먼저 token sequence에서 CLS token을 제거함
> - Concatenate: 나머지 tokens를 2D로 reshape함
> - Resample: 1x1 convolution 및 3x3 convolution(or deconvolution)을 적용하여 feature resolution (seqeunce length)와 channel dimension을 조절함
- 총 4개의 ViT layers에서 features를 추출함
- 4개의 features는 reassemble 연산을 통해 순차적으로 fuse됨 (FPN과 유사)
- 마지막으로 fuse된 features는 최종 dense prediction을 위해 사용됨

## 실험결과
- Depth estimation, semantic segmentation tasks에서 모두 좋은 성능 기록

## 의견
- / 