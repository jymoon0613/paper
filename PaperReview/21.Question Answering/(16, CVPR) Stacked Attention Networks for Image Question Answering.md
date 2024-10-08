# Stacked Attention Networks for Image Question Answering (CVPR, 2016)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Yang_Stacked_Attention_Networks_CVPR_2016_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/yang2016stacked.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Image question answering(QA)은 natural language questions에 대해 reference image의 content를 참조하여 자동적으로 답하도록 하는 task임
- Image로부터 question에 대한 answer를 찾기 위해서는 multi-step reasoning 과정이 필요함
> - Image에서 objects 및 question에서 언급된 concepts를 locate해야 하며, 점진적으로 불필요한 objects를 제거하고 answer를 도출하기 위해 가장 informative한 region에 집중해야 함
- 따라서, 본 논문에서는 image QA의 multi-step reasoning을 위한 stacked attention network(SAN)을 제안함
- SAN은 세 가지 주요 구성요소로 이루어져 있음
> - (i) CNN에 기반한 image model은 image로부터 high-level representations을 추출함
> - 이때 image의 각 region 별 하나의 feature vector가 추출됨
> - (ii) CNN 혹은 LSTM에 기반한 question model은 question으로부터 semantic vector를 추출함
> - (iii) Stacked attention model은 multi-step reasoning 과정을 통해 question과 연관된 image regions를 locate함(answer를 추론하기 위해)
> - 첫 번째 attention layer에서는 question vector를 사용하여 image vectors를 query하며, 두 번째 attention layer에서는 query의 결과로 반환된 image vectores와 question vector를 combine하여 refined query vector를 생성하고, 이를 기반으로 다시 image vectors를 query함
> - 최상단 attention layer의 image features를 마지막 query vector와 combine하여 answer를 예측함

## 접근법
- VGG-Net을 사용하는 image model은 입력 이미지로부터 feature maps를 추출함
> - ${3}\times{448}\times{448}$의 입력 이미지로부터 ${512}\times{14}\times{14}$의 feature maps를 출력
> - 이때 ${14}\times{14}$는 이미지의 regions 수이며(region의 크기는 $(448/14, 448/14) = (32, 32)$임), 각 region은 $512$ dimension의 feature vector로 표현됨\
> - 추출된 region feature vectors는 MLP를 거쳐 question vector와 같은 dimension으로 projection됨
- 이전의 연구들로부터 LSTM과 CNN 모두 texts로부터 semantic meaning을 추출하는 데 뛰어나다는 사실이 입증되었으며, 따라서 question model로 LSTM과 CNN 모두를 사용해봄
- LSTM을 question model로 사용하는 경우, embedding된 question의 각 word vector는 순차적으로 LSTM에 입력됨
> - 마지막 time step의 hidden layer ouput을 question vector로 사용함
- CNN을 question model로 사용하는 경우, 먼저 embedding된 question의 각 word vector는 모두 concat되고, concat된 word vectors에 convolution operation을 적용함
> - 1-3의 size를 지니는 3개의 convolution filters를 사용하며, 각각은 unigram, bigram, trigram에 대응됨
> - 3개의 convolution filters를 적용하여 얻은 각각의 feature maps에 max-pooling을 적용하고, 결과를 concat하여 최종적으로 question vector를 생성함
- Stacked attention module은 image feature matrix와 question vector를 입력으로 받아 multi-step reasoning 과정을 거쳐 answer를 예측함
- Answer는 주로 image의 fine-grained한 small region과 강하게 연관되어 있음
- 따라서, 하나의 global image feature만을 사용하여 answer를 예측하는 경우 answer와는 무관한 regions로 인해 noise가 발생할 수 있으며, 이는 부정확한 예측을 초래함
- SAN은 복수의 attention layers를 점진적으로 거치면서 reasoning을 수행하며, 따라서 점차 불필요한 noise를 제거하고 answer와 관련성이 높은 regions에 집중할 수 있음
> - 먼저, image feature matrix와 question vector를 MLP 및 softmax function에 입력하여 image feature matrix의 각 region에 대한 attention score를 계산함
> - Attention score를 사용하여 image feature matrix를 weighted sum함
> - Weighted sum된 feature vector에 question vector를 더해주어 refined query vector를 생성함
> - Refined query vector는 question information과 함께 answer와 관련된 visual information을 둘 다 담고 있음
> - 이러한 attention 과정을 여러번 반복함
> - 첫 번째 attention layer 이후, 두 번째 attention layer에서는 question vector가 refined query vector로 대체되며, 이후의 attention layers에서도 마찬가지로 이전 layer에서 계산된 refined query vector로 지속적으로 업데이트됨
> - 마지막 attention layer에서 계산된 refined query vector를 사용하여 answer를 예측함

## 실험결과
- 여러 image QA 데이터셋에서 좋은 성능 기록
- Stacked attention module의 경우 2개의 attention layer를 사용했을 때 성능이 가장 좋았음
- Stacked attention module의 attention maps를 시각화해본 결과, 첫 번째 attention layer에서는 question에서 언급된 concepts 및 objects에 주로 집중되어 있는 반면, 두 번째 attention layer에서는 answer 예측과 큰 관련이 있는 region으로 더욱 집중됨

## 의견
- / 