# Streaming Dense Video Captioning (CVPR, 2024)
[논문링크](https://openaccess.thecvf.com/content/CVPR2024/html/Zhou_Streaming_Dense_Video_Captioning_CVPR_2024_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhou2024streaming.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이상적인 video captioning 모델은 (i) video의 가능한 긴 input 시퀀스를 처리할 수 있어야 하며, (ii) 구체적인 묘사를 위해 긴 output 시퀀스를 처리할 수 있어야 함
> - 기존의 video captioning 모델은 둘 다 부족함
- 본 논문에서는 dense video captioning을 위한 streaming model을 제안함
- 제안하는 모델은 memory mechanism과 streaming decoding algorithm을 통해 위의 두 조건을 충족
> - Memory mechanism: 프레임을 하나씩 처리하면서 visual features를 지속적으로 업데이트
> - Streaming decoding algorithm: 모든 프레임을 처리한 뒤 captioning을 하는 기존(non-streaming models)과 달리, 정해진 timestamp마다 당시의 visual features로 target caption을 생성하도록 supervised됨
- 또한, 초반 timestamp에서 생성된 caption을 후반 timestamp의 caption 생성 시 제공하여 중복된 events를 예측하는 것을 방지하고, timestamp 상 멀리 떨어진 frames에 대한 context로서 사용함 (NLP의 long-term memory와 유사)

## 접근법
- Per-frame 단위로 처리하는 visual encoder와 autoregressive decoder를 사용함
- Memory mechanism은 K-means clustering에 기반하며, 입력 visual features에 대한 K개의 cluster centers가 memory features가 됨
> - 일정 timestamp마다 cluster centers, 즉, momory features를 업데이트함
- Streaming decoding algorithm은 특정 timestamp마다 당시까지 발견한 모든 events에 대한 caption을 생성함
- 또한, 두번째 decoding timestamp부터는 이미 생성된 이전의 caption을 참조하여 중복된 예측을 삭제함
> - 첫 번째 예측에서 caption $c1$, $c2$가 예측되었다면 두 번째 예측에서 $c1$, $c2$를 제외한 $c3$만이 새롭게 추가됨
> - 즉, 이전까지 예측된 captions 집합에 포함되지 않는 events에 대해서만 예측을 수행

## 실험결과
- ActivityNet, YouCook2, ViTT 데이터셋에서 검증함
- 세 개의 벤치마크 모두에서 기존 SOTA 대비 크게 향상된 성능을 기록함

## 의견
- /