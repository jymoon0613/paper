# Locality-Aware Generalizable Implicit Neural Representation (NIPS, 2023)

[논문링크](https://arxiv.org/abs/2310.05624)

<p align="center">
    <img width="800" alt='fig1' src="../img/lee2024locality.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Generalizable INR은 single coordinate-based MLP로 multiple data instances를 표현할 수 있다는 장점이 있지만 성능은 각 data instance마다 따로 훈련시키는 경우에 비해 떨어짐
- 저자들은 이러한 이유가 generalizable INR이 locality-awareness가 부족하기 때문이라고 주장함
> - 개별 데이터의 structure를 모델링하는 기능이 부족
- 본 논문에서는 locality-aware generalizable INR을 위한 encoder-decoder framework를 제안함
> - 데이터의 fine-grained details를 포착

## 접근법
- 본 논문에서는 Transformer encoder를 사용하여 각 데이터 instance의 latent code를 생성함
- Local information을 포함하는 generalizable INR을 생성하기 위해 locality-aware INR decoder를 사용함
> - Encoder에서 생성한 latent code와 input coordinate으로부터 계산된 frequency features의 cross-attention을 수행
- 여러 frequency bandwidths로 decoder의 output modulation features를 변환시키고, 다시 aggregate함

## 실험결과
- Image reconstruction, few-shot novel view synthesis에서 기존 방법들보다 좋은 성능 기록

## 의견
- /