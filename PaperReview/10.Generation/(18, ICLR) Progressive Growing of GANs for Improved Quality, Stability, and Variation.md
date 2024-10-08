# Progressive Growing of GANs for Improved Quality, Stability, and Variation (ICLR, 2018)

[논문링크](https://arxiv.org/abs/1710.10196)

<p align="center">
    <img width="600" alt='fig1' src="../img/karras2017progressive.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- High-resolution 이미지를 생성하는 것은 어려움
> - 이미지의 resolution이 클수록 생성 이미지와 원본 이미지를 구분하기 쉬워지고(디테일 차이가 뚜렷해짐), 따라서 그레디언트 문제가 급격히 증폭됨
> - 큰 resolution은 메모리 한계로 인해 작은 mini-batch를 사용해야 하며 이는 훈련의 안정성을 제한함
- 따라서, 본 논문에서는 generator와 discriminator를 점진적으로 키워가면서 학습시키는 방법을 제안함
> - 쉬운 low-resolution 이미지부터 시작
> - High-resolution 디테일 표현을 위한 레이어를 점진적으로 추가하면서 학습
- 제안하는 방법은 학습 속도를 매우 가속화하였으며 high-resolution 이미지에서의 안정성을 개선

## 접근법
- Generator와 discriminator는 low-resolution images에 대해 훈련된 후 점진적으로 layer가 추가되면서 high-resolution images에 대해 훈련됨
- 이는 이미지의 large-scale structure를 먼저 파악하고 finer-scale detail에 집중하도록 함
- 네트워크에 새로운 레이어를 추가할 때는 transition process를 추가하여 부드럽게 추가함: 이미 학습된 레이어에 갑작스러운 변화가 오는 것을 방지

## 실험결과
- 훈련 시간을 단축하면서 high-resolution 이미지 생성

## 의견
- /