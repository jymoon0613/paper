# Federated Multi-organ Segmentation with Inconsistent Labels (IEEE T-MI, 2023)

[논문링크](https://ieeexplore.ieee.org/abstract/document/10107904)

<p align="center">
    <img width="800" alt='fig1' src="../img/xu2023federated.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Medical image의 경우 법적인 제약 때문에 centralized된 learning paradigm을 적용할 수 없음
- 따라서, decentralized learning paradigm인 federated learning(FL)이 주로 사용됨
> - FL은 데이터 공유 없이 서로 다른 data-owners가 하나의 global model을 훈련시키도록 함
> - Server는 개별 client에서 locally 학습된 모델을 aggregate하여 global model을 업데이트 함
- 하지만, 여러 client에서 보유한 데이터는 annotation 방식이 다 다르며, 이는 label inconsistency를 유발할 수 있음
> - 구체적으로, 각 client가 가능한 전체 label이 아니라 일부의 label만 보유하고 있는 partial labels 문제가 생길 수 있음
- 따라서, 본 논문에서는 partial labels 문제를 해결하기 위해 federated multi-encoding U-Net(FED-MENU) method를 제안함

## 접근법
- 3개의 client nodes가 있고, 각 clinet는 CT dataset의 모든 labels 중 일부만을 보유하고 있는 FL 상황을 가정함
- Local client에서 훈련된 모델의 가중치는 각 client의 데이터셋 크기에 따라 weighted되어 global model에 반영됨
- 본 연구의 목적은 partial labels를 보유하고 있는 clients를 통해 전체 label에 대해 segmentation을 수행할 수 있는 global model을 훈련시키는 것
- 핵심 아이디어는 각 client가 본인들이 보유한 labeled organs에 대해서만 segment하도록 하는 것
- 이를 위해 모델의 multi-organ feature extraction을 각 organ에 specific한 feature extraction으로 decompose하는 multi-encoding U-Net(MENU-Net)을 제안함
- 구체적으로 MENU-Net은 M개의 sub-encoder와 하나의 shared decoder로 구성됨
> - M개의 sub-encoder 각각이 organ-specific한 feature extractor의 역할을 함
> - 하나의 이미지를 모든 sub-encoders에 전달하여 모든 organs에 대한 features를 병렬로 추출함
> - 추출된 features는 하나로 concat되어 shared decoder로 입력됨
> - Shared decoder는 multi-organ segmentation masks를 생성
- MENU-Net을 통해 분리된 sub-encoders가 각자의 client에 맞는 organ 특징을 학습할 수 있게됨
- 하지만, 반대로 sub-encoders는 각자의 client dataset만을 참조함으로써 domain-specific information을 포함하고 있을 수 있으며, 이는 이어지는 shared decoder에서 unfavorable한 요소로 작용함
> - 이상적인 시나리오는 각 sub-encoder가 domain과는 무관한 organ의 구조적 정보를 학습하는 것
- 따라서, 모든 sub-encoders에 공유되는 auxiliary generic decoder(AGD)를 사용하여 MENU-Net의 training을 regularize하고 organ-specific features를 향상시킴
- AGD는 모든 sub-encoders의 organs 정보를 학습하는 generic한 decoder이기 때문에, 각 sub-encoder는 AGD가 자신이 담당하는 organ을 잘 예측하도록 최대한 distinctive한 features를 전달해야 함

## 실험결과
- 6개의 public abdominal CT datasets에 대해서 평가함
- Centralized 및 federated 세팅 모두에서 성능이 좋았음

## 의견
- /