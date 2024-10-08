# Masked Autoencoders Are Scalable Vision Learners (CVPR, 2022)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2022/html/He_Masked_Autoencoders_Are_Scalable_Vision_Learners_CVPR_2022_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/he2022masked.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
### (1) Introduction 
- 하드웨어의 발전과 함께 최근의 Deep Learning 모델의 규모는 폭발적으로 증가해왔으며, 이러한 모델의 훈련에는 종종 공개적으로 사용할 수 없는 레이블이 지정된 수억 개가 넘어가는 이미지가 필요함 
- NLP에서는 이러한 데이터 부족 문제에 대응하여 Self-supervised Learning 기법을 사용했음 
- GPT의 Autoregressive Language Modeling과 BERT의 Masked Autoencoding은 입력 데이터의 일부를 제거하고 제거된 부분을 예측하는 간단한 Self-supervised 기법을 사용함 
- 이러한 방법은 천억개가 넘어가는 파라미터를 지니는 NLP 모델의 효과적 학습을 가능하게 했음 
- 이러한 Masked Autoencoding 방식은 컴퓨터 비전에도 적용할 수 있음 
- 하지만, BERT에서의 성공에도 불구하고 비전에서의 Autoencoding 방식의 발전은 NLP에 비해 상당히 뒤쳐져 있음 
- 그렇다면 Masked Autoencoding이 시각과 비전 간에 다른 점은 무엇인가? 
#### 1) NLP와 컴퓨터 비전에서 사용되는 모델의 아키텍처가 달랐음 
> - NLP는 Transformer 구조가 지배적이었지만, 컴퓨터 비전에서는 CNN이 지난 10년 동안 지배적이었음   
> - NLP의 Masked Autoencoding에서 사용되는 마스크 토큰 혹은 Positional Embedding을 CNN 구조에 통합시키는 것은 그렇게 간단하지 않았음  
> - 최근의 ViT 도입은 이러한 장애물을 해결 
#### 2) 시각과 언어는 정보 밀도 측면에서 차이가 있음 
> - 언어는 매우 의미론적이고 정보 밀도가 높음  
> - 하나의 문장에서 소수의 단어만 누락시켜도, 모델은 해당 단어들을 예측하기 위해 전체 문장에 대한 정교한 문맥적 이해를 필요로 함  
> - 반면에 이미지는 중복된 정보가 비교적 많음  
> - 따라서, 모델은 Parts, Objects, Scenes에 대한 의미론적, 문맥적 이해 없이 주변의 가시적 이미지 패치의 모양으로부터 쉽게 마스킹된 패치를 예측할 수 있음  
> - 본 논문에서는 매우 높은 비율로 이미지 마스킹을 진행하는 (75%) 간단한 트릭으로 이러한 문제를 해결함  
#### 3) Decoder는 언어와 시각을 재구성하는 데 서로 다른 역할을 함 
> - 비전에서 Decoder의 역할은 픽셀 단위로 마스킹된 패치를 재구성하므로 의미 수준이 낮음 (단순한 따라 그리기)   
> - 반면 NLP에서의 Decoder의 출력은 풍부한 의미 정보를 포함하는 누락된 단어  
> - 따라서, NLP에서의 Decoder 디자인은 성능에 크게 관련이 없지만, 이미지의 경우에는 Decoder를 어떻게 디자인하는지에 따라 학습된 잠재 표현 (Latent Representation)의 의미 수준의 Quality를 결정함  
- 이러한 분석에 따라, 본 논문에서는 비전에서의 표현 학습을 위해 간단하고, 효과적이고, 확장 가능한 형태의 Masked Autoencoder (MAE)를 제안함 
- 제안된 MAE는 이미지의 무작위한 위치를 마스킹하고, Pixel Space에서 가려진 부분을 재구성함 
- MAE의 Encoder와 Decoder는 Asymmetric함 
> - Encoder는 오직 가시적인 부분만 처리함 (마스킹된 부분은 처리 X) 
> - Decoder는 비교적 가벼우며 주어진 잠재 표현과 마스크 토큰을 바탕으로 마스킹된 입력을 재구성함 
> - 이러한 구성은 계산량을 크게 줄여줌 
> - 또한, 매우 높은 비율로 마스킹하는 것도 Encoder의 계산량을 줄이는 효과가 있음 
- MAE는 잘 일반화되는 고용량 모델을 학습함 
- MAE는 개선된 일반화 성능으로 ImageNet-1K에서도 ViT-Large/Huge 같은 매우 큰 모델을 학습시킬 수 있음 
### (2) Related Work 
- GPT와 BERT에서 사전 훈련 방법으로 사용한 Masked Language Modeling 및 Autoregressive 방식은 NLP에서 매우 성공적이었으며 이러한 방법은 입력 시퀀스의 일부를 유지하고 누락된 콘텐츠를 예측하도록 모델을 훈련시킴 
- 여러 증거를 통해 이러한 사전 훈련 방식이 다양한 Downstream Tasks에도 잘 일반화된다는 것을 증명함 
- Autoencoding은 표현 학습을 위한 고전적인 방법이며, 입력을 잠재 표현에 매핑하는 Encoder와 이를 바탕으로 입력을 재구성하는 Decoder 구조로 구성됨 
- Denoising Autoencoders (DAE)는 입력 신호를 손상시키고 손상되지 않는 원래 신호를 재구성하는 방법을 학습하는 Autoencoder 구조임 
- 여러 방법들은 픽셀 마스킹, 색상 채널 제거와 같은 다양한 손상 상황에서 일반화된 DAE로 취급될 수 있으며, MAE 또한 DAE의 한 형태이지만 여러 면에서 기존의 DAE와 다름 
- Masked Image Encoding은 마스킹으로 인해 손상된 이미지에서 표현을 학습함 
- Context Encoder는 CNN 네트워크에서 큰 누락 영역을 Inpainting함 
- NLP의 성공으로부터 영향을 받아 최근에는 ViT 기반으로 진행됨 
- Self-supervised Learning은 컴퓨터 비전에서 최근 관심 받는 분야이며 사전 훈련을 위한 다양한 작업이 제시되어 왔음 
- 최근의 대조 학습 방법과 Autoencoding은 개념적으로 다른 방향을 추구함 

## 접근법
### (1) Overview 
- MAE는 제한된 입력으로부터 원본 이미지를 재구성하는 간단한 접근법을 지님 
- Encoder는 관찰된 신호를 잠재 표현으로 매핑하고, Decoder는 잠재 표현으로부터 원본 이미지를 재구성함 
- 기존의 Autoencoder 구조와 달리, MAE의 Encoder는 오직 가시적인 부분만을 처리하고, Decoder는 비교적 가벼우며 Encoder가 매핑한 잠재 표현과 마스크 토큰을 통해 원본 이미지를 생성한다는 점에서 Asymmetric한 구조를 지님 
### (2) Masking 
- 기본적인 Vi	T에서처럼, MAE는 입력 이미지를 겹치지 않는 여러 개의 패치로 구분하고, Random Sampling 방식을 통해 정해진 비율의 이미지 패치를 마스킹함 
- 높은 마스킹 비율을 선택하여 이미지의 중복을 크게 제거함으로써 모델이 단순히 인접 패치의 모양을 모방하여 예측하지 못하도록 함 
- 또한, Uniform Distribution은 이미지 중심에 마스크 패치가 쏠리는 현상을 방지하며, 큰 마스킹 비율을 선택하는 것은 효율적인 인코더 설계에 기여함 
### (3) MAE Encoder 
- MAE의 Encoder는 ViT이며, 마스킹된 이미지에서 오직 가시적인 부분만 처리함 (Unmasked Parts) 
- ViT와 완전히 유사하게 이미지 패치의 Projection과 Positional Embedding의 추가, 연속된 Transformer Block 연산을 통해 진행 
- 가시적인 부분만 처리하므로 Computational Cost 및 Memory 측면에서 매우 효율적임 
### (4) MAE Decoder 
- Decoder의 입력으로 들어오는 것은 Encoder에 의해 Encoding된 가시적 패치와 마스킹된 부분에 대한 마스크 토큰으로 구성된 전체 토큰 세트임 
- 각 마스크 토큰은 예측해야 하는 누락된 패치의 존재를 나타내는 학습된 공유 벡터임 
- 이러한 전체 토큰 세트에 Positional Embedding을 추가하는데, 이러한 Positional Embedding이 없으면 마스크 토큰이 이미지에서 해당 위치에 대한 정보를 표현할 수 없음 
- Decoder는 일련의 Transformer Block으로 구성 
- Decoder는 사전 훈련 단계에서만 사용되기 때문에 Encoder와 독립적인 방식으로 유연하게 설계할 수 있음 
- Decoder는 각 토큰에 대해 Encoder보다 약 10% 적은 계산만 하도록 경량화됨 
- 이러한 비대칭 디자인은 사전 훈련 시간을 크게 단축시킴 
### (5) Reconstruction Target 
- MAE는 마스크된 각 패치의 픽셀값을 예측하여 입력을 재구성함 
- Decoder의 최종 출력은 하나의 패치를 표현하는 픽셀값 벡터이고, 출력 채널 수가 패치의 픽셀 수와 동일함 
- Decoder의 출력은 재구성된 이미지를 표현하도록 Reshape됨 
- BERT와 같이, 오직 마스킹된 위치에서만 Loss를 계산함 
### (6) Simple Implementation 
- 구현이 매우 간단함 
- 사용되는 Shuffling 및 Unshuffling은 매우 빠르며 오버헤드가 적음 

## 실험결과
### (0) Overview 
- ImageNet-1K 데이터셋에서 사전 학습시키고, End-to-End Fine-tuning (FT) 및 Linear Evaluation Protocol (LP) 진행 
### (1) Main Properties 
#### 1)	Masking Ratio 
> - 마스킹 비율이 75%일 때 FT와 LP 모두 성능이 가장 좋았음 
> - 이는 15%를 사용하는 BERT와 차이가 있으며, 20%와 50% 정도를 사용하는 이전의 비전 연구와도 차이가 있음 
> - 모델은 누락된 패치를 추론하여 그럴듯한 출력을 생성함 
> - 모델은 단순히 선이나 질감을 모방하여 완성할 수 없는 사물과 장면의 형태를 이해하며, 저자들은 이러한 추론 과정이 유용한 표현 학습과 연결되어 있다고 주장 
> - LP에 비해 FT는 Masking Ratio에 덜 민감함 
#### 2)	Decoder Design 
> - Decoder의 깊이와 너비가 깊을수록 LP의 성능이 좋아짐 (Decoder Block의 개수, Dimension) 
> - Decoder의 깊이와 너비는 FT에서 크게 상관이 없음 (작아도 좋음) 
#### 3)	Mask Token 
> - MAE의 핵심적인 디자인은 마스크 토큰을 Encoder에서는 사용하지 않고, Decoder에서만 처리하는 것 
> - Encoder가 마스크 토큰을 사용하면 성능이 오히려 감소함 
> - Encoder가 마스크 토큰을 입력으로 받으면 실제 이미지에는 존재하지 않는 정보를 같이 인코딩하는 것이므로 인코딩 정확도를 감소시킬 수 있음 
> - 따라서, 마스크 토큰을 Encoder에서는 처리하지 않음으로써 인코더가 항상 실존하는 이미지 패치만을 볼 수 있도록 제안하여 정확도 개선 
> - 또한, Encoder에서의 계산량과 메모리 소비량을 효과적으로 줄일 수 있음 
#### 4)	Reconstruction Target 
> - Normalization이 된 픽셀 단위 예측을 사용할 경우 성능이 가장 좋았음 
> - 픽셀 단위 예측이 토큰 자체를 예측하는 방식보다 성능이 좋고 간단했음 
#### 5)	Data Augmentation 
> - MAE에 별다른 Data Augmentation을 적용하지 않고, Crop만 사용했을 때도 성능이 좋았음 
> - 또한, Data Augmentation을 사용하지 않는 경우에도 성능이 좋았는데, 이는 많은 종류의 Data Augmentation이 필수적인 Contrastive Learning과 비교했을 때 독보적임 
> - Random Patch Masking 자체가 Data Augmentation의 효과를 지니기 때문인 것으로 보임 
#### 6)	Mask Sampling Strategy 
> - 큰 Block 단위로 마스킹하는 경우 일정 비율 (50%)까지는 성능이 괜찮았지만, 그 이상을 마스킹할 경우 (75%) 성능이 크게 감소함 
> - 일정한 간격의 Grid 단위로 마스킹하는 경우, 비교적 쉽기 때문에 Loss는 작지만 Representation Learning 퀄리티는 매우 떨어짐 
> - 제안하는 Random Sampled Masking 방식이 가장 효과적이었음 
#### 7)	Training Schedule 
> - 사전 훈련 Epoch가 늘어날수록 성능이 선형적으로 증가함 
> - 1600 Epoch까지 학습하여도 여전히 증가 
> - 이는 어느 정도의 Epoch 이후 성능이 포화되는 Contrastive Learning과는 다름 
### (2) Comparison with Previous Results 
- 다른 SSL 방법의 성능을 뛰어넘었음 
- 기존의 ViT-L 모델은 ImageNet-1K 데이터셋에서는 성능이 낮았지만, 제안하는 MAE는 ImageNet-1K에서도 ViT-L 모델 성능이 좋았으며 이는 MAE가 일반화에 더 유리함을 증명함 (즉, 데이터 제한적인 상황에서 모델의 크기를 키우는 데 더욱 적합) 
### (3) Partial Fine-tuning 
- LP와 FT 외에도 상위 레이어 몇 개를 훈련시키는 Partial Fine-tuning (PFT) 성능도 평가함 
- 몇 개의 블록만 Fine-tuning해도 전체를 Fine-tuning한 것만큼 좋은 성능 달성 
- 이는 MoCo v3의 경우를 뛰어넘음 
### (4) Transfer Learning Experiments 
- 여러 Downstream Tasks에서 적용한 결과를 평가함 
- Object Detection, Semantic Segmentation, Classification 모두에서 좋은 성능을 보여줌 

## 의견
- 간단하지만 ViT를 사전 학습시킬 수 있는 좋은 방법을 제시 
- 다양한 측면에서 각 기능들의 의미와 효과를 증명함 
- Context Encoder → MAE 대체? 