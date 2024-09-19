# Bridging Generalization Gaps in High Content Imaging Through Online Self-Supervised Domain Adaptation (arXiv, 2023)

[논문링크](https://arxiv.org/abs/2311.12623)

<p align="center">
    <img width="500" alt='fig1' src="../img/haslum2024bridging.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 high content imaging(HCI) 환경에서의 domain adaptation 기법을 다룸
> - HCI: biological image의 high content data 추출을 목적으로 하는 이미지
> - 생화학(biochemistry), 세포 생물학(cellular biology), 미생물학(microbiology), 분자 생물학(molecular biology), 신약 발견(drug discovery) 등에서 사용
- HCI 환경에서는 generalization gap 문제가 자주 발생함
> - 실험 조건, 측정 장치의 차이 등으로 인해 발생
- 이를 해결하기 위해 online self-supervised domain adaptation(SSDA)인 cross-batch online domain adaptation(CODA)를 제안함

## 접근법
- 이전의 domain adaptation 접근법과 마찬가지로 SSDA 기법 중 하나인 dual-model approach를 사용함
> - 우선 feature extractor를 DINO로 훈련시킨 뒤, classifier를 attach하여 fine-tuning함
> - 이후 out-of-domain samples에 대해서는 classifer를 freeze시키고 feature extractor만 update함
- 하지만 HCI 데이터에 dual-model approach를 바로 적용시키면 성능이 매우 낮았음
> - Biological signals가 experimental artifacts에 의해 가려지는 경우가 있기 때문
- 따라서 기존 SSDA를 변형하여 이러한 artifacts에 agnostic한 adaptations를 수행함
> - Cross-domain consistency learning(CBCL)을 사용함
> - CBCL은 서로 다른 batch(domain)에서 같은 treatment를 적용한 두 views 간의 feature similarity를 최대화
> - CBCL을 DINO pre-training stage, domain adaptation stage에 적용하여 feature robustness를 향상시킴

## 실험결과
- 실험 결과 CODA를 적용한 경우의 domain generalization 성능이 가장 좋았음

## 의견
- /