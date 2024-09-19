# Token Contrast for Weakly-Supervised Semantic Segmentation (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Ru_Token_Contrast_for_Weakly-Supervised_Semantic_Segmentation_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/ru2023token.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Weakly-supervised semantic segmentation(WSSS)은 image-level labels 등 weak한 annotations만 사용하여 pixel-level predictions를 수행함
- 최근의 WSSS 기법들은 class activation map(CAM)을 pseudo labels로 사용하여 훈련함
- 하지만 CNN 기반의 CAM을 사용하는 경우, convolution은 local features만을 모델링하므로 전체적인 object regions를 activate할 수 없어 부정확하다는 한계가 있음
- 따라서, 본 논문에서는 self-attention을 통해 global feature interactions를 모델링할 수 있는 ViT 기반의 CAM을 사용하여 문제를 해결하고자 함
- 하지만, ViT의 연속적인 sefl-attention은 spatial smoothing operations로 작용되어 ViT의 모든 patch tokens가 uniform해지는 over-smoothing 문제를 야기함
> - Over-smoothing은 patch tokens의 activation에 기반하여 pseudo labels를 생성해야 하는 WSSS 태스크에 있어서 치명적임
- 따라서, over-smoothing 문제를 해결하기 위한 patch token contrast(PTC) module과 class token contrast(CTC) module을 제안함
- 최종적으로 PTC/CTC 모듈을 기반으로한 WSSS 프레임워크인 token contrast(ToCo)를 제안함

## 접근법
- ViT의 self-attention 연산이 누적될수록 모든 patch tokens가 uniform해지는 over-smoothing이 일어나지만, 초기의 layers는 semantic diversity를 보존하고 있음
- 따라서, PTC 모듈을 통해 최종 layer의 patch tokens를 intermediate layers에 대해 supervision하는 방식으로 over-smoothing 문제를 해결하고자 함
> - Intermediate layers의 output patch tokens가 auxiliary classification head로 입력되고, classification 결과에 대해 CAM을 계산
> - 추출된 CAM에 대해 thresholds를 적용하여 foreground, background, uncertain regions로 구성된 pseudo token label 생성
> - 생성된 pseudo token label의 각 category(foreground, background만 고려)로부터 pairwise relations를 계산하고, final patch tokens를 supervision함
- CAM의 discriminativeness를 더 개선하기 위해 CTC 모듈을 적용
> - PTC 모듈에서 식별된 uncertain region 패치를 일부 샘플링하여 local tokens를 구성함
> - Pseudo token label을 기준으로 local tokens를 positive/negative tokens로 구분함
> - Global class token과 local tokens를 projection하고 InfoNCE loss를 적용함
- 제안하는 ToCo로 ViT를 훈련시키는 동시에, ViT를 pseudo-label generator로 사용하여 WSSS task를 수행하는 single-stage 방식을 사용함

## 실험결과
- ToCo 프레임워크로에서 훈련된 ViT가 생성한 pseudo-label의 퀄리티는 다른 WSSS 기법에서 제안되었던 것보다 뛰어났음
- ToCo의 single-stage WSSS는 다른 single-stage WSSS 기법과 비교했을 때 가장 좋은 성능을 기록함

## 의견
- /