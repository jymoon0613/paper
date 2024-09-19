# Junction Tree Variational Autoencoder for Molecular Graph Generation (ICML, 2018)

[논문링크](https://proceedings.mlr.press/v80/jin18a.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/jin2018junction.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 molecular graph 생성을 위한 새로운 generative 모델을 제안
> - Molecules를 graph로 처리
- Variational autoencoder(VAE)를 확장하여 모델 설계
> - VAE + junction tree (JT-VAE)

## 접근법
- Graph encoder로는 graph message passing network 사용
> - Nodes와 edges의 features를 모두 고려
- Molecular graph와 tree 구조로 변경한 molecules를 서로 다른 VAE로 각각 입력
- Tree 구조의 molecules encoding은 Tree decoder로 입력
> - Tree decoder로 molecular graph를 생성
> - 기존 molecular graph와 생성된 graph 간의 유사도 계산

## 실험결과
- CVAE, GVAE 등의 이전 molecule reconstruction 모델과 비교했을 때 JT-VAE가 더 좋은 성능을 기록
> - JT-VAE는 항상 valid한 molecules를 생성 (존재가능한 분자 구조)

## 의견
- /