# VL-Adapter: Parameter-Efficient Transfer Learning for Vision-and-Language Tasks (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Sung_VL-Adapter_Parameter-Efficient_Transfer_Learning_for_Vision-and-Language_Tasks_CVPR_2022_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/sung2022vl.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Vision and languagge(V&L) 모델은 downstream tasks를 위한 fine-tuning 과정을 거치지만, 최근의 거대한 모델 크기는 이 과정에서 메모리 및 속도 측면에서 여러움이 있음
- 이러한 문제를 해결하기 위해 parameter-efficient한 training methods가 제안되었고, 이 중 adapter를 이용하는 방식이 있음
> - Adapter는 모델의 중간 layers에 추가되어 adapter만 fine-tuning하더라도 전체 모델을 fine-tuning하는 것만큼 유의미한 성능을 달성함
- 하지만, adapter 모듈을 V&L tasks에 적용된 적은 거의 없으며 V&L tasks의 경우 vision 및 language 정보를 fusion하는 과정도 필요하기 때문에 더욱 복잡함
- 따라서, 본 논문에서는 adapter-based parameter-efficient training techniques를 V&L 모델에 적용해보고자 함

## 접근법
- 본 논문에서 사용하는 V&L 모델은 CLIP과 BART로 구성됨
> - CLIP-ResNet101와 BART-base 모델을 사용
> - CLIP의 output representations를 visual projection layer를 통해 language model의 차원으로 변환시키고, 변환된 visual representations와 sentence representations를 concat하여 encoder-decoder 모델(BART)에 입력함
- V&L 모델의 목표는 입력 이미지(혹은 비디오)와 문장이 주어졌을 때 모델의 output text와 정답 text와의 agreement를 최대화하는 것
- Adapter는 모델의 모든 attention layers 및 feed-forward layers 다음에 삽입됨
> - CLIP은 제외
- Adapter 학습 방식을 더 개선하기 위해 hypernet, compactor 등과 같은 기법도 적용해봄
> - 결과적으로 각 tasks에서 서로 다른 adapters를 훈련시키는 multiple adapters, 모든 tasks에서 adapter의 일부 파라미터를 공유하는 half-shared adapters, 모든 tasks에서 모든 adapter 파라미터를 공유하는 single adapters의 variants를 만들 수 있음

## 실험결과
- 여러 V&L tasks (visual question answering, visual reasoning, image captioning, video question answering, video captioning)에서 평가함
- 실험 결과 single adapters 구조를 사용했을 때 성능이 다른 두 개에 비해 좋았음

## 의견
- /