# Gazeformer: Scalable, Effective and Fast Prediction of Goal-Directed Human Attention (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Mondal_Gazeformer_Scalable_Effective_and_Fast_Prediction_of_Goal-Directed_Human_Attention_CVPR_2023_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/mondal2023gazeformer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Human gaze(시선)를 예측하는 것은 사용자의 의도를 예상한다는 측면에서 interactive system 디자인에서 매우 유용함
- 본 논문에서는 visual search 상황에서의 시선 고정을 예측하는 데 집중함
> - 이미지를 입력으로 받아 target-object category에 따른 sequence of eye movements를 예측
- 본 논문에서는 zero-shot learning을 gaze prediction problem으로 확장한 ZeroGaze 환경을 정의함
> - Training 시 고려하지 못한 target-object category에 대해서도 gaze를 예측
- ZeroGaze 환경을 위한 Gazeformer를 제안함
> - Gazeformer는 scalable함
> - 특정 target-object category에 대해 backbone detector를 학습시킬 필요가 없음
> - 대신 language embedding을 사용하여 image-target representations를 모델링함
> - 어떠한 target categories라도 language embedding으로 변환할 수 있으므로 training에서 고려하지 못했던 target에 대한 예측이 가능

## 접근법
- ResNet-50 backbone으로부터 입력 이미지의 visual features 추출
- RoBERTa 모델로부터 target의 semantic embedding을 추출
- 두 features를 concat하고 linearly projection하여 joint features를 계산
- 이후 $L$ 길이의 scanpath sqeuence를 예측하기 위해 $L$개의 fixation quries를 정의하고, joint features와 함께 Transformer decoder layers에 입력
> - Fixation quries는 random하게 초기화된 learnable embeddings
> - Self-attention을 통해 각 fixation embeddings 간의 interactions가 모델링되며, encoder-decoder cross-attention을 통해 joint features와의 interactions도 모델링됨
- $L$ 길이 time steps의 raw fixation coordinates를 예측함
> - $i$번째 fixation의 좌표 $x_i, y_i$, fixation duration $t_i$를 예측
> - 또한 MLP layer를 추가하여 valid한 fixation인지에 관한 $v_i$도 예측 (sequence 길이를 맞추기 위해 padding한 fixation일 수 있음)

## 실험결과
- ZeroGaze 및 일반적인 Gaze prediction 환경에서 모두 뛰어난 성능 달성

## 의견
- /