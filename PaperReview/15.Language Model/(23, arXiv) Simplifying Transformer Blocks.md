# Simplifying Transformer Blocks (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2311.01906)

<p align="center">
    <img width="500" alt='fig1' src="../img/he2023simplifying.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 기존 Transformer block을 간소화시키고자 함

## 접근법
- 기존 Transformer block이 self-attention과 channel MLP가 순차적으로 연결되었다면, 간소화된 block에서는 병렬적으로 처리됨
- 또한, 모든 skip-connections와 normalization layers를 제거함
- 기존 self-attention을 shaped attention으로 대체함
> - Attention score에 identity matrix를 더해주어 각 토큰이 자기 자신에 대한 attend를 강화하고, signal propagation 능력을 향상시킴
- Shaped attention 시 value를 사용하지 않으며 self-attention output에 대한 projection layers도 사용하지 않음

## 실험결과
- 기존 Transformer block보다 적은 파라미터와 빠른 처리 속도로 더 좋은 성능 달성

## 의견
- /