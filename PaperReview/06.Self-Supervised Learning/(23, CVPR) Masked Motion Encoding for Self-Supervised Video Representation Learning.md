# Masked Motion Encoding for Self-Supervised Video Representation Learning (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Sun_Masked_Motion_Encoding_for_Self-Supervised_Video_Representation_Learning_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/sun2023masked.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- MIM을 video domain으로 확장하려는 시도들이 있음 (masked video modeling, MVM)
- 하지만 MAE도 high masking ratio에도 불구하고 reconstruction을 어느 정도 잘 수행하듯이, video의 temporal한 정보가 실제로는 학습과정에서 무시될 수 있음
> - Video의 temporal한 특성 없이 각 frame 수준에서 reconstruction을 수행
- 또한, 기존의 시도들은 고정된 stride로 sparese하게 추출된 frames에서 MVM을 수행하므로 fine-grained motion details를 학습하기 위한 supervision signals를 제공하기 어려움
> - 세밀한 동작 세부 정보는 서로 다른 동작을 구분하는 데 매우 중요함
- 본 논문에서는 위의 두 문제를 해결하기 위한 새로운 mask-and-predict 패러다임을 제안함
> - Apperance contents를 예측하는 것이 아닌 position과 shape의 변화를 의미하는 motion trajectory를 예측
> - Masked motion encoding(MME)

## 접근법
- MME는 reconstruction target을 static appearance에서 object motion information(position과 shape의 변화)으로 변경
- 우선 VideoMAE와 마찬가지로 모든 frame에 적용되는 공통의 mask map을 생성하여 적용함
- 또한, MAE와 마찬가지로 unmasked patches를 Transformer encoder로 인코딩하고 decoder에는 인코딩된 unmasked patches와 MASK patches를 같이 입력함
- Decoder는 masked patches의 'motion trajectory'를 reconstruct하도록 훈련됨
> - 인간이 shape와 position의 변화에 따라 moving objects를 인식하는 것처럼 이 두 요소를 motion trajectory를 표현하는 데 사용함
> - Position features는 직전 frame과 비교했을 때의 position 변화로 표현됨
> - Shape features는 서로 다른 frame의 tracked object의 HOG descriptor임

## 실험결과
- Video representation learning에서 VideoMAE보다 좋은 성능 기록

## 의견
- /