# Learning a Deep Embedding Model for Zero-Shot Learning (CVPR, 2017)

[논문 링크](https://openaccess.thecvf.com/content_cvpr_2017/html/Zhang_Learning_a_Deep_CVPR_2017_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2017learning.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 딥러닝은 object recognition 성능을 매우 끌어올렸지만, 이는 상당히 많은 labeled 데이터를 요구함
- 의자와 같은 일상적인 object는 labeled 데이터를 구하기 쉬운 반면, 다른 많은 objects들은 labeled 데이터를 구하기 힘듦
- 이러한 경우 대다수의 모델은 labeled 데이터가 적은 sample에 대해서는 recognition을 잘 못함
- 하지만, 인간은 어떠한 visual samples를 보지 않아도 object를 recognize할 수 있는 특성이 있음 (Zero-Shot Learning (ZSL))
- 예를 들어, 얼룩말을 한 번도 본 적이 없는 사람이라도 말을 본 적이 있고 얼룩말이 검은색-흰색 줄무늬가 있는 말이라는 사실을 알고 있다면 인식할 수 있음
- 이러한 인간의 ZSL 능력을 machine에 인식하려는 노력이 활발히 진행되었음
- ZSL은 seen classes로 이루어진 훈련 데이터에 대한 학습과 unseen classes가 어떻게 seen classes와 의미론적으로 연과되는지에 의존함
- Seen classes와 unseen classes는 고차원의 벡터 공간인 semantic space에서 관계가 정의되며, seen classes에 대한 지식이 unseen classes로 전이됨
- Semantic spaces로서 가장 처음으로 도입된 것은 semantic attributes를 사용하는 방법임
- 이러한 방법들은 각 class name을 해당하는 attributes 목록에 의해 벡터로 표현하며 이는 class prototype으로 불림
- 이후에는 semantic word vector space, sentence descriptions/captions을 사용하는 방법이 제안되기도 하였음
- Semantic word vector space은 class name을 word vector로 projection하여 서로 다른 class 간의 비교가 가능하도록 하는 방법이며, sentence descriptions/captions은 neural language model을 통해 이미지 description을 vector representation으로 projection하는 방법임
- 정리하자면, ZSL은 (1) class prototype과 visual feature vectors가 project되는 joint embedding space를 학습하고, (2) 학습된 embedding space에서의 nearest neighbor search를 통해 unseen class prototype과의 최적 매핑을 찾음으로써 수행될 수 있음
- 이전까지의 대부분의 방법들은 visual features를 인코딩하기 위해 pre-trained된 deep CNN 모델을 사용함
- 하지만 이러한 방법들은 찾은 visual features로 어떻게 embedding space를 찾을 것인가에 대해서는 상이하기 때문에 end-to-end learning으로 구현되지 못함
- 따라서, 본 논문에서는 end-to-end로 학습하는 deep embedding based ZSL model을 제안함
- 이러한 방식은 (1) end-to-end 최적화가 더 좋은 embedding space 학습을 가능하게 한다는 점과, (2) neural network 기반의 joint embedding model이 multi-task learning 혹은 multi-domain learning과 같은 transfer learning task를 위한 유연함을 제공해준다는 점과, (3) multiple semantic spaces가 가능하다면, 해당 모델은 multiple modalities를 혼합할 수 있는 매커니즘을 제공할 수 있다는 점에서 이점이 있음
- Deep embedding model을 사용하는 ZSL에서는 embedding space 선택이 중요함
- 대부분의 Deep embedding model들은 semantic space를 embedding space로 사용하는데 이는 많은 data points (or hubs)가 일부 특정 unseen class prototypes와 매칭되는 hubness problem을 야기할 수 있음
- Semantic space를 embedding space로 사용한다는 것은 visual feature vectors가 semantic space로 투영되어야 함을 의미하며, 이는 투영된 데이터 포인트의 분산을 축소하여 hubness problem을 악화시킴
- 따라서, 본 논문에서는 hubness problem을 해결하기 위해 output visual feature space of a CNN subnet을 embedding space로 사용하며, 이전의 방법들과는 반대로 attribute or word vector가 visual feature space로 투영됨 (방향 반대)
- 또한 간단하면서 효과적인 multi-modality fusion method를 통해 semantic space representation에 대한 end-to-end learning이 가능하도록 함

## 접근법
- 제안하는 방법은 CNN subnet을 통해 D차원의 visual feature를 출력하는 visual encoding branch와 class의 semantic embedding을 담당하는 semantic encoding subnet으로 구성됨
- D차원의 visual feature space가 image content와 semantic representation의 embedding space가 됨
- Semantic encoding subnet은 L차원의 class semantic representation을 입력으로 받아 fc layer 및 ReLU를 거쳐 D차원의 semantic embedding vector를 출력
- 두 벡터 (visual feature, semantic embedding vector)의 MSE를 계산하여 discripency를 최소화하고 두 branch를 연결
- Inference 시에는 주어진 unseen image의 visual feature와 모든 unseen class의 semantic embedding vector와의 거리 기준으로 가장 가까운 class로 예측
- 두 가지 semantic representation을 동시에 사용하는 경우 (attribute vector + word vector) 구조가 살짝 변경됨 (fusion)
- Text description을 semantic representation에 사용하는 경우 bidirectional RNN을 사용하며 역시 구조가 살짝 변경됨 (multi-modal도 가능)
- Bidirectional RNN도 end-to-end로 학습 가능
- Ridge regression을 사용한 예시를 통해 class semantic representation space의 variance가 visual feature space의 variance의 분산보다 작으며, 따라서 원점과 더 가깝다는 것을 증명
- 즉, semantic space로 class semantic representation space를 사용하는 경우 hubness problem이 발생할 가능성이 커짐

## 실험결과
- AwA, CUB-200-2011, ImageNet-2010-1K, ImageNet-2012/2010을 통해 검증함
- 모든 데이터셋에서 zero-shot 성능을 개선함
- Semantic space를 무엇으로 설정하는가가 성능에 영향을 미치는 것을 확인

## 의견
- /