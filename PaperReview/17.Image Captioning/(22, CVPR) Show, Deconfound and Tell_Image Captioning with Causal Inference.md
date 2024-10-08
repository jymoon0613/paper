# Show, Deconfound and Tell: Image Captioning with Causal Inference (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Liu_Show_Deconfound_and_Tell_Image_Captioning_With_Causal_Inference_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/liu2022show.png?raw=true"></br>
    <em><font size=2>Illustration of the CIIC framework for image captioning.</font></em>
</p>

## 연구목적
- Image captioning 분야에서는 encoder-decoder 아키텍처가 주로 사용됨
  - Encoder는 입력 이미지로부터 visual features를 추출하고, Decoder는 visual features로부터 적절한 caption을 생성
- 이후의 발전 방향은 (i) 입력 이미지의 시각적 특징을 더 잘 학습하는 방법과 (ii) inter-/intra-model interactions 강화하기 위한 아키텍처 개선에 집중되어 있음
- 하지만 두 방향 모두 visual/linguistic confounder에 의해 발생하는 spurious correlations를 경계해야 할 필요가 있음
  - ex) '케이크'와 '포크'가 같이 등장하는 입력이 많다면, 모델은 '케이크'의 시각적 정보 혹은 '케이크'의 word embedding만 참고하고도 '포크'의 존재를 예측할 수 있음(short-cut 발생)
  - 이는 모델이 '케이크'와 '포크' 사이의 spurious correlations을 학습하도록 하여 '케이크'와 함께 '숟가락'이 등장하더라도 모델은 여전히 '포크'를 예측할 가능성이 높음
  - 이때 '케이크'의 시각적 특성이 visual confounder가 되며, '케이크'의 word embedding은 linguistic confounder가 됨
- 본 논문에서는 이러한 문제를 해결하기 위해 causal inference에 기반한 image captioning 방법을 제안함(causal inference based image captioning, CIIC)
  - CIIC는 interventional object detector(IOD)와 interventional Transformer decoder(ITD)로 구성됨

## 접근법
- CIIC는 Transformer기반의 encoder-decoder 구조이며, causal inference는 visual representation step과 sentence generation step에서 각각 수행됨
- Encoder에서, Faster R-CNN은 이미지의 object visual features를 추출하여 Transformer encoder로 전달함
- 이때 visual spurious correlations를 제거하기 위해 IOD를 같이 사용하며, IOD는 같은 이미지로부터 disentangled된 object visual features(IOD features)를 Transformer encoder로 전달함
- 두 pipeline으로부터 전달된 features는 self-/cross-attention을 거쳐 통합된 representation으로 매핑됨
- Decoder에서는 visual/linguistic confounder를 동시에 다루기 위한 ITD가 제안됨
- ITD는 기존 Transformer decoder block을 베이스라인으로 하되, FFN 모듈 다음에 casual intervention(CI) block을 추가함
- CI는 IOD와 마찬가지로 visual/lingustic dictionary를 사용하여 순차적으로 단어를 예측함

## 실험결과
- MS COCO 데이터셋(Karpathy test split)에서 SOTA 성능 달성
- 생성된 captions의 bias degree를 평가한 결과 CIIC는 다른 모델에 비해 biased caption이 훨씬 적었음

## 의견
- /