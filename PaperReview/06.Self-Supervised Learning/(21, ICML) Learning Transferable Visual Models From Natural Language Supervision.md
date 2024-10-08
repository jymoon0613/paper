# Learning Transferable Visual Models From Natural Language Supervision (ICML, 2021)

[논문링크](https://proceedings.mlr.press/v139/radford21a)

<p align="center">
    <img width="800" alt='fig1' src="../img/radford2021learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Raw text를 사용하여 pre-training하는 방식은 NLP 분야에서 큰 성과가 있었음
- 하지만, computer vision 분야에서는 여전히 high-quality crowd-labeled 데이터로 pre-training하는 방법이 주로 사용되고 있음
- Text로부터 image representations를 학습하고자 하는 시도는 여럿 있었지만 여전히 SOTA 모델에 비해서는 성능이 매우 떨어졌음
- 낮은 성능의 원인은 scale이었으며, 기존 SOTA 모델들은 엄청난 양의 이미지를 오랜 시간동안 학습했음
- 본 논문에서는 이러한 gap을 줄이고자 image classifiers를 large-scale language supervision으로 학습시키고자 함 (contrastive language-image pre-training, CLIP)
- Language supervision으로 학습하는 것은 classic한 task-specific labels를 필요로 하지 않으므로 모델 scale에 유리함
- 또한, language supervision으로부터의 학습은 모델이 단순한 image representation이 아닌 image representation과 language의 연결 관계를 학습하게 하여 훨씬 flexible한 representation learning이 가능함

## 접근법
- 대규모 학습 데이터셋을 구성하기 위해 우선 MS-COCO, Visual Genome, YFCC100M에서 natural language titles나 English descriptions이 있는 샘플만 추출함
  - 대략 ImageNet 데이터셋 정도의 샘플만 남음
- 추가로 인터넷에 공개된 소스를 통해 4억 개의 image-text pairs로 구성된 데이터셋을 수집 (WebImageText, WIT)
  - 각 이미지는 영어 title 혹은 description이 같이 주어짐
- 초기 CLIP은 CNN/ViT와 text Transformer를 동시에 훈련하며 주어진 이미지의 caption을 예측하도록 훈련됨
- 하지만, 이러한 훈련 방식은 훈련 데이터셋 증가에 따른 효과적인 scaling이 불가능했음
- 따라서, image encoder와 text encoder 간의 contrastive learning으로 image-text pairs에 대한 pre-training 수행
- 즉, 주어진 이미지의 caption을 한 단어씩 정확하게 예측하는 것이 아니라 caption을 하나의 text 뭉치로서 해당 caption이 어떤 이미지와 pair가 되어야 하는가를 예측함
  - 이전 훈련 방식에 비해 4배 더 효과적인 scaling이 가능했음
- 구체적으로, CLIP은 $N$ 개의 image-text pairs가 주어졌을 때 ${N}\times{N}$ 개의 가능한 pairings를 예측함
- 하나의 batch 내에서 $N$ 개의 올바른 image-text pairs 간의 embeddings의 cosine similarity가 최대화되도록 하며, $N^2-N$ 개의 incorrect pairs의 embeddings 간에는 최소화되도록 함
- 이를 위해 CLIP은 image encoder와 text encoder가 동시에 매핑되는 multi-modal embedding space를 학습해야 함

## 실험결과
- CLIP으로 학습된 모델의 zero-shot performance를 평가함 (Zero-shot CLIP)
  - 주어진 dataset의 모든 target classes를 text input으로 사용하여 embeddings를 추출하고, 주어진 image embedding과의 cosine similarity를 계산한 뒤, cosine similarity가 가장 큰 class로 예측 수행
- ImageNet에서 supervised하게 훈련된 ResNet-50의 features를 입력으로 받는 linear classifier를 fully-supervised하게 훈련시켜서 비교함 ('Linear Probe on ResNet50')
- Zero-shot CLIP은 27개의 vision task 데이터셋 중 16개에서 fully-supervised 'Linear Probe on ResNet50'보다 좋은 성능을 기록했음 
- 반면, Zero-shot CLIP은 lymph node tumor detection (PatchCamelyon), satellite image classification (EuroSAT) 등 복잡한 vision tasks에 대해서는 성능이 낮았음 
  - 하지만 이러한 task는 관련 경험이 없는 인간도 매우 하기 어려운 task이므로 zero-shot transfer 성능에 대한 논의가 무의미할 수 있음
- 다음으로는 supervised(ImageNet)/self-supervised(SimCLR-v2)하게 훈련된 네트워크(ResNet-50, BiT-M)의 features를 입력으로 받는 linear classifier를 few-shot 방식으로 훈련시켜 비교함 ('Few-shot Linear Probes')
- Zero-shot CLIP은 대부분의 'Few-shot Linear Probes'보다 성능이 좋았으며, few-shot CLIP은 'Few-shot Linear Probes'보다 성능이 훨씬 좋았음
- CLIP의 linear probe 성능은 대부분의 vision tasks에서 기존 SOTA 모델들의 성능을 능가했음
- 또한, CLIP은 distribution shift에도 훨씬 robust했음

## 의견
- /
*/