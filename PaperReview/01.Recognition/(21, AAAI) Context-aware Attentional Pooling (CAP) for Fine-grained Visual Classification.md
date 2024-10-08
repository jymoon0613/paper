# Context-aware Attentional Pooling (CAP) for Fine-grained Visual Classification (AAAI, 2021)

[논문 링크](https://ojs.aaai.org/index.php/AAAI/article/view/16176)

<p align="center">
    <img width="600" alt='fig1' src="../img/behera2021context.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이미지 분류에서 CNN의 좋은 성과는 주로 질감과 모양에서 구별되는 Object의 포즈와 부분 정보를 분리하여 이미지 내용을 추론하는 CNN 모델의 능력에서 비롯됨 
- 대부분의 Discriminative한 피처는 주로 전체적인 모양 혹은 모양의 변화에 기반하지만, 이러한 피처들은 미묘한 시각적 차이를 반영하지 못하여 더욱 세분화된 하위 범주를 구분해내는 데 부적합함 
- 이를 해결하기 위해, 이미지의 미묘한 차이가 인식될 수 있도록 여러 지역적 부분으로부터의 피처들과 이러한 피처들의 계층 구조를 관망하여 추론하는 Global Descriptor가 필수적이며, 이러한 Global Descriptor는 또한 중요한 부분을 강조하는 역할을 할 수도 있음 
- 현재까지의 Fine-grained Image Classification은 모델의 시야를 제안하는 방식을 통해 모델이 이미지의 지역적인 부분들에 집중하도록 하는 것을 강조했지만, 이미지에 대한 풍부한 표현을 학습하기 위해서는 모델의 시야를 제한하는 것뿐만 아니라 모델이 하나의 픽셀로부터 Object, Scene으로의 Image Generation 과정을 이해하도록 해야함 
- 즉, 단순히 이미지의 분할을 통해 부분적인 피처를 감지하는 것뿐만 아니라, 각각의 부분이 갖는 의미와 중요성을 여러 Partial Descriptor를 통해 파악하는 것이 중요하고, 이러한 각각의 Partial Descriptor는 서로의 정보를 공유하고 보완하면서 Object, Image Scene에 대한 완전한 기술이 가능하도록 함 
- 따라서, 본 연구에서는 Context-aware Attentional Pooling (CAP)를 제안하여 이미지 내의 여러 지역의 모습과 그 공간 배열을 효과적으로 인코딩함 
- CAP Module은 Base CNN으로부터 Convolution 피처를 입력으로 받아 Object와 지역적 부분 내의 계층 구조를 설명하기 위해 여러 통합된 영역의 Latent Representation을 강조하는 방법을 학습함 

## 접근법
- CAP은 Base CNN의 Feature Map을 입력으로 받음 
- 먼저, CAP은 주어진 Feature Map의 Pixel-level Similarity를 학습함 (Self-attention 사용) 
- Attention 이후, 문맥적 정보를 효과적으로 학습하기 위해 Multiple Integral Regions 방법을 사용함 
- 모든 픽셀 위치에서, W와 H를 바꿔가며 다양한 크기의 이미지 패치를 생성 (Different Location, Size and Aspect) 
- 이는 통합적인 문맥적 표현이며 이미지의 작은 변화로 표현되는 풍부한 문맥적 추출을 잡아낼 수 있도록 함 
- 생성한 모든 이미지 패치에 대해 Bilinear Pooling을 사용하여 동일한 크기의 Feature Map을 추출함 
- 추출한 각 패치의 Feature Map을 바탕으로 다시 Self-attention을 진행하여 문맥적 정보를 추출함 
- 이는 각 패치가 자기 자신을 비롯한 주변과 어떤 관련성이 있는지를 모델링함 
- 추출된 각 이미지 패치의 Context Vector는 Attention과 Saliency를 모델링한 결과임 
- 이후에는 Spatial Structure Encoding을 진행함 
- Spatial Structure Encoding은 LSTM 모델을 사용하여 이미지 패치 간의 Spatial Structure를 추출하는 단계 
- 마지막으로, Soft Clustering 기법을 사용하여 분류를 수행함 

## 실험결과
- 여러 Fine-grained Image Classification에서 SOTA 성능을 기록함 

## 의견
- 학습 과정이 매우 복잡 
- 모든 이미지 패치를 추출하는 것은 Computational Cost가 매우 높음 