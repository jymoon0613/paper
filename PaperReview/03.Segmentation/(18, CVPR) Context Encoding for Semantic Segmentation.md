# Context Encoding for Semantic Segmentation (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhang_Context_Encoding_for_CVPR_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2018context.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- CNN 기반의 segmentation 모델들은 spatial resolution의 손실을 막기 위해 dilated convolution과 같은 방법들을 사용함
- 하지만, 이러한 방법들은 종종 pixels을 global scene context로부터 고립시켜서 pixels의 misclassification을 유발함
- 이에 최근의 방법들은 multi-resolution의 pyramid-based representation을 사용하여 receptive field를 키우는 방법을 사용함
- 하지만 여전히 context representation은 명시적으로 다루어지지 않으며 receptive field의 증가가 contextual information을 포착하기 위한 최선의 방법인지는 명확하지 않음
- 따라서, 본 논문에서는 global scene context 정보를 활용하기 위한 context encoder module을 제안함
> - Context encoding module은 semantic encoder loss(SE-loss)를 사용함
> - Context encoding module은 encoding layer를 통해 global context를 포착하며 class-dependent한 feature maps를 선택적으로 강조함 (e.g., indoor scene에서 각 pixel이 'vehicle' class로 할당되는 영향을 최소화)
- 이러한 context encoding module을 바탕으로 새로운 semantic segmentation framework인 context encoding network(EncNet)을 제안함

## 접근법
- Encoding layer는 backbone에서 처리된 feature map($C\times{H}\times{W}$)으로부터 encoded semantics라고 불리는 global semantic context를 포착함
- 이후 Encoded semantics로부터 context 정보 반영을 위한 scaling factors를 예측함 (channel attention을 위한 soft mask와 유사, ($C\times1\times1$))
- Encoding layer의 input feature maps와 scaling factors를 channel-wise multiplication하여 encoding layer의 최종 output을 계산
- 하지만, 일반적인 semantic segmentation training의 경우 독립적인 per-pixel 예측만이 고려되므로, 네트워크는 context를 이해하는 데 어려움이 있을 수 있음
- 따라서, context encoding module의 학습을 regularize하기 위해 네트워크가 global semantic information 이해하도록 하는 semantic encoder loss(SE-loss)를 사용함
> - Encoded semantics에 추가적인 fully conntected layers를 부착하여 주어진 장면에 어떤 object categories가 존재하는지 예측하도록 함
> - Binary cross-entropy를 사용하여 최적화함
- Context encoding module을 기반으로한 context encoding network(EncNet)를 설계함
> - 이전 연구들과 마찬가지로 pre-trained backbone의 일부 stage에서 dilated conv. 사용
> - Segmentation을 위해서는 FCN framework를 사용함

## 실험결과
- PASCAL-Context dataset, PASCAL VOC 2012 dataset에서 SOTA 달성

## 의견
- /