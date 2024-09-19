# Visual Saliency Transformer (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Liu_Visual_Saliency_Transformer_ICCV_2021_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/liu2021visual.png?raw=true"></br>
    <em><font size=2>Overall architecture of our proposed VST model for both RGB and RGB-D SOD.</font></em>
</p>

## 연구목적
- Salient object detection (SOD)란 영상 속에서 가장 두드러지고 attention-grabbing한 object를 찾아내어 해당 객체의 전체 범위를 segment하는 task, 즉, background로부터 foreground 객체를 분할하는 task를 의미함
- 기존 SOD 분야에서 제안된 모델은 대부분 CNN에 기반함
- 본 논문에서는 SOD를 seq2seq 관점에서 재정의하여 Transformer 기반의 SOD 모델인 Visual Saliency Transformer(VST)를 제안함
> - RGB, RGB-D 환경에서 unified하게 사용가능함

## 접근법
- VST는 Transformer encoder, Transformer convertor, multi-task Transformer decoder로 구성됨
- Transforemr encoder는 사전 훈련된 T2T-ViT 모델을 backbone으로 사용함
> - RGB 환경에서는 단일 T2T-ViT backbone으로 이미지 토큰을 모델링함
> - RGB-D 환경에서는 이미지 토큰과 함께 depth map으로부터의 토큰을 모델링하는 T2T-ViT 모델을 추가로 사용
- Encoder 토큰을 decoder space로 매핑하기 위해 encoder와 decoder 사이에 convertor 모듈을 추가함
> - RGB 환경에서는 간단한 Transforemr layers를 사용함
> - RGB-D 환경에서는 image 토큰과 depth 토큰을 fusion하기 위해 서로의 query를 변경하여 self-attention을 수행하는 cross-modality attention을 적용함
- Decoder는 처리된 토큰을 saliency map으로 변환시키는 역할을 함
> - Downsampled된 토큰을 다시 upsampling하기 위해 T2T 연산을 역으로 적용하는 reverse T2T 변환을 수행함
> - 그 결과, 토큰은 입력 이미지 크기와 동일하게 upsample됨
- Saliency detection과 boundary detection을 동시에 학습하는 multi-task decoder를 구성하여 성능을 끌어올림

## 실험결과
- VST는 RGB 및 RGB-D 환경의 SOD task에서 기존 CNN 모델들을 제치고 SOTA를 달성함

## 의견
- /