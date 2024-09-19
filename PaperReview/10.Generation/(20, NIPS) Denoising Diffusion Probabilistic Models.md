# Denoising Diffusion Probabilistic Models (NIPS, 2020)

[논문링크](https://proceedings.neurips.cc/paper/2020/hash/4c5bcfec8584af0d967f1ab10179ca4b-Abstract.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/ho2020denoising.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 diffusion probabilistic models를 발전시켜서 다른 생성 모델보다 뛰어난 high-quality images를 생성하고자 함
- 생성에 활용되는 조건부 확률 분포 $P_{\theta}(x|z)$를 학습하기 위해 Diffusion Process $q(z|x)$를 활용
> - $x$는 입력 이미지, $z$는 가우시안 노이즈, $\theta$는 모델의 파라미터

## Background
### (1) Markov Chain
- Markov Chain이란 Markov 성질을 갖는 이산 확률과정
> - 이산 확률과정: 이산적인 시간(0초, 1초, ...) 속에서의 확률적 현상
> - Markov 성질: 특정 상태의 확률(t+1)은 오직 현재(t)의 상태에 의존함
> - 즉, $P(S_{t+1}|S_t)=P(S_{t+1}|S_1, ..., S_t)$
### (2) Diffusion Model
- Diffusion model은 학습된 데이터의 패턴을 생성하는 역할을 함
> - 패턴 생성 과정을 학습하기 위해 고의적으로 noise를 추가하고(forward/diffusion process), 이를 다시 복원하는(reverse process) 조건부 PDF를 학습
- Forward process $q(x_t|x_{t-1})$로부터 조건부가 바뀐 reverse process $q(x_{t-1}|x_{t})$는 inference 과정에 활용할 수 없음
- 다만, $q(x_t|x_{t-1})$가 gaussian이면, $q(x_{t-1}|x_{t})$도 gaussian이라는 점은 이미 증명됨
- 따라서, $p_{\theta}(x_{t-1}|x_{t})$를 상정해 $q(x_{t-1}|x_{t})$를 근사(approximation)함
> - 즉, Diffusion model은 $p_{\theta}(x_{t-1}|x_{t}) \approx q(x_{t-1}|x_{t})$가 되도록 학습
- 두 개의 각 process(forward, reverse) 내 변화 과정은 Markov Chain으로 매우 많은 단계로 쪼개져 구성됨
### (3) Forward(Diffusion) Process
- 주어진 이미지 $x_0$에 서서히 noise를 추가하는 과정 ($q$)
- $x_0$에 noise를 추가하여 $x_1$을 만드는 것을 $q(x_1|x_0)$으로 표현할 수 있음
> - 이를 time $t$에 대하여 general하게 표현한다면 $q(x_t|x_{t-1})$로 표현 가능
- 마지막 time step의 결과 $x_T$는 완전히 destroy된 형태가 됨
> - 즉, $N(x_T;0,I)$를 따름
- Forward process에서 주입되는 gaussian noise의 크기는 사전적으로 정의되고, 이를 $\beta_{t}$로 표기
### (4) Reverse Process
- Reverse process는 Diffusion process의 역과정인 denoising 과정으로, gaussian noise를 제거해가며 특정한 패턴을 만들어가는 과정 ($p$)
> - $x_t$가 들어왔을 때 $x_{t-1}$을 예측할 수 있다면 $x_0$도 예측할 수 있음
- 이는 mean과 variance를 파라미터로 갖는 가우시안에 대해 이루어짐
> - $p_{\theta}(x_{t-1}|x_{t}) \coloneqq N(x_{t-1};\mu_{\theta}(x_t,t), \Sigma_{\theta}(x_t,t))$
> - 이떄 $\mu_{\theta}(x_t,t), \Sigma_{\theta}(x_t,t)$는 학습의 대상
- Objective function $L$은 생성된 이미지의 negative log-likelihood를 최소화
> - $\mathit{\mathbb{E}}[-\log{p_{\theta}(x_0)}]$를 최소화
> - 이때 variational bound는 $\mathit{\mathbb{E}}_{q}[-\log{\frac{p_{\theta}(x_{0:T})}{q(x_{1:T}|x_0)}}]$이 되고, 따라서, $\mathit{\mathbb{E}}_{q}[-\log{p(x_T)}-\sum_{1\leq{t}}{\log\frac{p_{\theta}(x_{t-1}|t_t)}{q(x_t|x_{t-1})}}]\coloneqq{L}$을 최소화
> - 이를 다시 정리하면,  $\mathit{\mathbb{E}}_{q}[D_{\text{KL}}(q({x_T|x_0})~||~p(x_T))+\sum_{1<t}{D_{\text{KL}}(q(x_{t-1}|x_t,x_0)~||~p_{\theta}(x_{t-1}|x_t))}-\log{p_{\theta}(x_0|x_1)}]$가 됨
> - 이때 $D_{\text{KL}}(q({x_T|x_0})~||~p(x_T))$는 $L_T$, $\sum_{1<t}{D_{\text{KL}}(q(x_{t-1}|x_t,x_0)~||~p_{\theta}(x_{t-1}|x_t))}$는 $L_{t-1}$, $-\log{p_{\theta}(x_0|x_1)}$는 $L_0$을 의미

## 접근법
- DDPM논문의 핵심은 reverse process의 $L_{t-1}$을 예쁜 형태로 다시 정리한 것
- Diffusion model의 objective function $L$에서 가장 중요한 term은 $L_{t-1}$임
> - $x_0$부터 시작하여 conditional하게 식을 전개하다보면, tractable한 forward process posterior $q(x_{t-1}|x_t,x_0)$의 정규분포를 알 수 있는데, 이를 바탕으로 KL-divergence를 계산하면 결과적으로 학습하고자 하는 $p_{\theta}(x_{t-1}|x_t)$를 학습시킬 수 있음
- 본 논문에서는 forward process의 variances인 $\beta$를 learnable한 파라미터로 두는 것이 아니라 상수로 fix하기 때문에 $L_T$는 고려하지 않아도 됨
- $L_0$는 두 normal 분포 사이의 KL-divergence이기 때문에 간단하게 계산 가능
- $L_{t-1}$을 계산하기 위해서는 첫 번째로 $q(x_{t-1}|x_t,x_0)$의 분포를 알아내고, 두 번째로 $p_{\theta}(x_{t-1}|x_t)$를 알아내기 위해 $\mu_{\theta}(x_t,t), \Sigma_{\theta}(x_t,t)$를 알아내야 함

## 실험결과
- High-quality images 생성

## 의견
- /