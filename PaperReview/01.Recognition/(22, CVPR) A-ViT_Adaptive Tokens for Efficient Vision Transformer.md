# A-ViT: Adaptive Tokens for Efficient Vision Transformer (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Yin_A-ViT_Adaptive_Tokens_for_Efficient_Vision_Transformer_CVPR_2022_paper.html?ref=https://githubhelp.com)

<p align="center">
    <img width="800" alt='fig1' src="../img/yin2022vit.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 ViT의 compuational complexity를 줄이기 위해 입력 이미지의 복잡도에 따라 자동으로 연산량을 조절하는 방법을 다룸
> - 오직 discriminative한 tokens만 처리함
> - 'halt the computation for different tokens at different depth'
> - 모델 내부적으로 구현하여 파라미터 추가가 없음

## 접근법
- ViT의 각 레이어에서 적응적으로 tokens의 사용 여부를 결정하기 위해 토큰마다의 halting probability(or score)를 계산함
- 레이어가 진행됨에 따라 halting score는 토큰마다 축적되며, 누적된 halting score가 특정 임계값을 넘으면 해당 토큰을 사용하지 않음 (token stopping)
> - Token stopping이 되면 해당 token의 값을 0으로 만들어주고, self-attention 과정에서 mask out해줌
> - 마지막 레이어에서는 모든 토큰이 stopping됨
- Halting score 계산은 매우 간단하며 파라미터가 없음
> - 각 레이어에서, 각 토큰의 첫번째 dimension value를 두 scalar parameters(constants) beta와 gamma로 scaling을 해주고 sigmoid를 적용함
> - Halting score를 포함한 효과적 수렴을 위해 각 token의 stopping layer와 halting score remainder를 기반으로한 ponder loss 정의
- 최종 예측에는 mean field formulation을 적용함
> - 모든 layer에서의 CLS token를 halting score로 weighted sum한 최종 CLS token을 최종 예측에 사용
- 최종 loss function은 classification loss와 ponder loss의 합으로 구성됨 (ponder loss는 별도의 hyperparameter로 조절)
- 이때 모델은 ponder loss의 hyperparameter의 값에 따라 훈련이 매우 불안정할 수 있음
- 따라서, tokens가 target depth에서 stop되도록 halting scores를 regularize할 수 있는 distributional prior를 제안함 (이미지별 variation은 유지함)
- 각 레이어의 halting score를 평균하여 구한 halting score distribution에 대해 gaussian distribution으로 정의된 prior halting score distribution과의 KL-divergence를 계산함
- 최종 loss function은 이러한 distribution loss도 합친 것으로 정의됨

## 실험결과
- Layer가 진행될수록 가장 salient한 토큰만이 남음
- 적은 정확도의 희생으로 FLOPs 크게 낮출 수 있었음

## 의견
- /