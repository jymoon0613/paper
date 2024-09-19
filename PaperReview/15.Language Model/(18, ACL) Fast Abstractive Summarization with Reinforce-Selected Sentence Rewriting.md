# Fast Abstractive Summarization with Reinforce-Selected Sentence Rewriting (ACL, 2018)

[논문링크](https://arxiv.org/abs/1805.11080)

<p align="center">
    <img width="600" alt='fig1' src="../img/chen2018fast.png?raw=true"></br>
    <em><font size=2>Our extractor agent.</font></em>
</p>

## 연구목적
- Document summarization 태스크는 크게 (1) extractive와 (2) abstractive의 두 접근법으로 나뉘어지며, 최근에는 abstractive 접근법이 크게 발전함
> - Extractive: 입력 document에서 salient한 sentences를 선택하여 출력함
> - Abstractive: document에 대한 summary를 직접 생성함
- Abstractive는 더 정확한 summary를 제공하지만, 긴 document에 대해서는 느리게 동작하며 결과도 부정확할 수 있음
- 또한 abstractive는 multi-sentence summary를 생성해야 할 때 redundancy 문제, 즉, 중복된 내용의 summary를 생성하는 문제가 있음
- 본 논문에서는 이러한 두 문제를 해결하기 위해 extractive-abstractive가 결합된 하이브리드 아키텍처를 설계함
> - Extractor는 salients한 문장을 선별하고, abstractor는 선별된 문장을 rewrite하여 summary를 생성함
> - 두 아키텍처를 결합하기 위해 policy-basaed reinforcement learning(RL)을 사용함

## 접근법
- Extracor는 document로부터 salient한 sentences를 추출하기 위해 우선 document 내의 모든 sentence에 대한 representations를 인코딩
> - 인코딩 이후 Bi-RNN으로 representations를 추가적으로 모델링
- Sentece representations는 LSTM-RNN으로 입력되어 salient한 sentences가 선택됨
- Abstractor는 선택된 sentences를 기반으로 정확한 summary sentence를 생성함
> - Abstractor는 encoder-aligner-decoder를 사용함
- Extractor는 non-differentiable한 hard selection 연산을 진행하므로 대응 방법이 필요
- 이를 위해 extractor와 abstractor를 서로 다른 objective로 훈련시키고, RL이 full-model end-to-end 학습을 위해 적용됨
> - Extractor objective: salient한 sentences를 선택
> - Abstractor objective: 요약된 summary를 생성
- Extractor는 ground-truth summary sentence와 가장 유사한 document sentence를 찾도록 훈련됨
- Abstractor는 일반적인 sequence-to-sequence 방식으로 입력 document sentence로부터 ground-truth summary sentence를 생성하도록 훈련됨
- Policy-based RL은 다음과 같이 동작함
> - Extractor가 좋은 sentence를 선택 → abstractor는 ground-truth와 잘 매칭되는 summary를 생성 → 높은 score 보상
> - Extractor가 나쁜 sentence를 선택 → abstractor는 ground-truth와 잘 매칭되지 않는 summary를 생성 → 낮은 score로 보상

## 실험결과
- CNN/Daily Mail 데이터셋에서 SOTA 성능 달성

## 의견
- /