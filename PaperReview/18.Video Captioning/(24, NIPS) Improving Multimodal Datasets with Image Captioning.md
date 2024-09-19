# Improving Multimodal Datasets with Image Captioning (NIPS, 2024)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2023/hash/45e604a3e33d10fba508e755faa72345-Abstract-Datasets_and_Benchmarks.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/nguyen2024improving.png?raw=true"></br>
    <em><font size=2>Raw captions crawled from the web contain significant noise; cosine similarity filtering helps
reduce noise but discards many images that are useful for training.</font></em>
</p>

## 연구목적
- 최근 웹에서 수집한 대규모 image-text pairs로 multi-modal 모델을 pre-training 시키는 것은 성능을 위한 기본적인 과정으로 여겨지고 있음
- 하지만 raw web data는 noisy하고 uninformative할 수 있기 때문에 복잡한 필터링 과정을 통해 전처리되며, 이 과정에서 수집한 데이터의 60%-90%가 제거됨
  - 필터링 조건은 image aspect ratio, 캡션 길이 등 다양함
  - 최근에는 CLIP 모델을 사용하여 image와 text(caption) 간의 align 정도를 측정하여 필터링하기도 함
- 본 논문에서는 synthetic captions를 사용하여 이러한 제거되는 examples의 활용성을 증진시키고자 함

## 접근법 및 실험 결과
- DataComp 벤치마크에서 NSFW 필터링 등 최소한의 필터링만을 적용한 데이터를 사용함
- Captioning 모델로는 BLIP, BLIP2, OpenCLIP-CoCa를 사용함
- 해당 captioning 모델로 synthetic captions을 생성한 뒤, CLIP 모델을 해당 데이터에 대해 훈련시킴
- 데이터의 양이 samll이나 medium인 경우 synthetic captions를 사용하는 것이 성능에 긍정적인 영향을 주었음
- 반면에 데이터의 양이 large로 이미 많은 경우 model-generated synthetic captions와 web-scraped text 간의 차이가 더 커져 성능이 부정적 영향을 주었음

## 의견
- /