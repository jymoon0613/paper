# Explain Me the Painting: Multi-Topic Knowledgeable Art Description Generation (ICCV, 2021)

[논문링크](https://openaccess.thecvf.com/content/ICCV2021/html/Bai_Explain_Me_the_Painting_Multi-Topic_Knowledgeable_Art_Description_Generation_ICCV_2021_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/bai2021explain.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문에서는 paintings에 대한 풍부한 descriptions를 자동으로 생성하는 방법을 제안함
> - 일반적인 image captioning과 달리 배경지식, 작가에 대한 디테일한 설명, 작업 과정의 맥락 등에 대한 포괄적인 설명이 필요함
> - 일반적인 image captioning이 content에만 집중했다면 paintings에 대한 descriptions는 content, form, context를 포괄적으로 다루어야 함
- 따라서, (i) description generation process에 external knowledge를 도입하고, (ii) multi-topic language model을 사용하여 painting의 서로 다른 측면을 설명하는 multi-topic and knowledgeable art description generation framework를 제안함

## 접근법
- 제안하는 framework는 (i) masked sentence generation, (ii) knowledge retrieval, (iii) knowledge extraction and filling으로 구성됨
- MSCOCO 데이터셋은 general captions 생성을 학습하는데 효과적이지만, MSCOCO에서 훈련된 decoder는 sparse하게 등장하는 entities를 생성하는 데 여러움이 있음
> - 본 task의 경우 artistname, location 등이 매우 sparse할 것
- 따라서, masked sentence generation 과정에서는 decoder가 오직 masked sentences만을 생성하도록 하고, masked된 부분에 대해서는 추후에 external knowledge로 채워넣는 방식을 채택함
> - Artworks에 대한 descriptions로 구성된 corpus에 대해 named-entity-recognition(NER)을 사용하여 entities를 식별하고, 식별된 entities를 entity tags(e.g. person)로 대체하여 masked sentence generation 훈련을 위한 데이터셋을 생성
> - LSTM 기반의 decoder를 사용하되, topic에 따라 서로 다른 decoder를 정의하고 선택적으로 사용함
> - 혹은 topic label을 deocder의 입력으로 사용하여 하나의 decoder만 사용하는 방법도 있음
> - Decoder는 ResNet으로 추출한 visual features를 입력으로 받음
> - 그 결과, 서로 다른 topic에 대한 서로 다른 masked sentences를 생성할 수 있음
- External knowledge를 사용하기 위해 DrQA를 사용하여 Wikipedia에서 관련된 정보를 retrieval함
> - 먼저, visual features에 대해 multi-task attribute prediction model을 사용하여 artist, type, timeframe, school 속성을 예측함
> - 또한, Visual Genome 데이터셋에서 사전 훈련된 object detection 모델을 사용하여 painting에서 visual concepts를 추출함
> - 추출된 attributes와 visual concepts를 query로 사용하여 knowledge retrieval을 수행
> - 상위 5개의 articles를 출력
- Masked senetence를 external knowledge로 채워넣음
> - Retrieval한 5개의 articles에 다시 NER을 적용하여 entities를 식별함
> - 사전에 추출된 attributes와 articles의 식별된 entities로 candidate wods를 만듦
> - BERT 기반의 sentence-to-sentence 모델을 통해 masked sentence의 가려진 부분을 candidate words로 채워넣도록 훈련함

## 실험결과
- SemArt 데이터셋 사용하여 평가함
> - SemArt에 포함된 original comments를 topic 별로 구분하는 전처리 수행
- 생성된 descriptions의 퀄리티를 평가하기 위해 human evaluation 수행
> - Painting, 생성된 description, original SemArt comment, title, artist, creation year를 보여주고 정성적인 평가 진행
> - Understandable, relevance, vericity, content existence, form existence, context existence 항목에 대해 점수를 매김
- 기존 baseline에 비해 understandable 항목을 제외하면 모두 높은 평가를 받았음
> - Understandable의 경우 baseline이 훨씬 짧은 description을 생성하기 때문인 것으로 보임

## 의견
- /