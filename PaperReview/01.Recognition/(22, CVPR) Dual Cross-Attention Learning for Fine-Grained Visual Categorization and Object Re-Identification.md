# Dual Cross-Attention Learning for Fine-Grained Visual Categorization and Object Re-Identification (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/Zhu_Dual_Cross-Attention_Learning_for_Fine-Grained_Visual_Categorization_and_Object_Re-Identification_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhu2022dual.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Self-attention은 하나의 입력 시퀀스에서 각 position을 연관 짓고 global dependency를 추출하는 방법
- NLP에서의 성공을 넘어 self-attention은 2D 이미지를 패치 단위 시퀀스로 취급하는 최근의 vision transformer에서도 성능 향상을 이끔
- 본 연구에서는 self-attention module을 보다 확장해서 fine-grained objects를 식별하기 위한 subtle feature embeddings을 잘 학습하도록 하는 방법을 제안함
- 이를 위해 global images와 local high-response regions 간의 interactions을 강화하기 위한 global-local cross-attention (GLCA)을 제안함
- Fine-grained image recognition을 위한 또 다른 방법으로는 두 이미지 간의 차이를 비교하여 subtle visual difference를 찾는 pair-wise learning이 있음
- 이러한 pair-wise learning을 위한 pair-wise cross-attention (PWCA)도 제안함
- 제안하는 두 가지 타입의 cross-attention 방법은 (dual cross-attentiol learning, DCAL) 기존의 self-attention과 호환이 가능하며 쉽게 구현 가능함

## 접근법
- 먼저 global-local cross-attention이 global images와 local high-reponse regions의 interaction을 강조하기 위해 제안됨
- 먼저 high-response regions을 찾기 위해 matrix multiplication을 통한 누적 attention score를 계산하여 aggregated attention map을 추출함
- Aggregated attention map의 첫 번째 행은 CLS 토큰의 누적 weights를 의미함
- CLS 토큰의 누적 weights에 대해 R개의 highest response인 query vetors를 선택함
- 선택된 query vectors와 모든 key, value vectors를 사용하여 cross-attention 수행 (GLCA)
- GLCA는 fine-grained recognition을 위한 discriminative spatial regions를 강조하는 효과를 지님
- 또한 query vectors는 선택된 것만 사용하고 key와 value vectors는 전체를 사용함으로써 보다 global한 관점에서 high-reponse 영역을 추출함
- Pair-wise cross-attention은 training set으로부터 랜덤하게 추출된 두 이미지 간의 cross-attention을 수행함
- 한 이미지의 query vectors와 두 이미지의 key, value vectors를 합한 key, value vectors로 cross-attention 수행
- PWCA는 계산 복잡도를 고려하여 training 시에만 적용

## 실험결과
- CUB, Stanford Cars, Aircrafts로 구성된 fine-grained object recognition task 및 VeRi-776, MSMT17, Market1501, DukeMTMC로 구성된 object re-identification task에서 성능 평가
- 일부 데이터셋을 제외한 대부분의 경우 SOTA 성능 달성

## 의견
- /