# GCNet: Non-local Networks Meet Squeeze-Excitation Networks and Beyond (ICCV, 2019)

[논문링크](https://openaccess.thecvf.com/content_ICCVW_2019/html/NeurArch/Cao_GCNet_Non-Local_Networks_Meet_Squeeze-Excitation_Networks_and_Beyond_ICCVW_2019_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/cao2019gcnet.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Local neighborhood pixels의 연관성만을 모델링하는 CNN의 한계를 보완하기 위해 non-local network(NLNet)는 self-attention mechanism을 통해 long-range dependencies를 모델링함
> - NLNet의 non-local operation은 한 pixel position의 output response를 계산할 때 해당 position과 feature map 상의 모든 pixel positions 사이의 연관성을 반영함
> - 이때 각각의 pixel position 별로 계산되는 global한 연관성은 하나의 pixel poisition에 대해 해당 position과 feature map 상의 다른 pixel positions가 얼마나 중요하게 연관되어 있는지를 나타내는 일종의 query-specific attention weights이라고 볼 수 있음
- 하지만, 본 논문의 저자들이 서로 다른 pixel positions에서 query-specific attention weights을 시각화해본 결과 attention weights의 분포가 거의 동일했음
> - 즉, non-local operation은 query-independent dependency를 학습했음
- 따라서, 저자들은 non-local block을 간소화하여 모든 pixel position에 대해 하나의 query-independent attention map을 공유하도록 함
> - 연산량이 크게 줄었으며 성능 감소는 거의 없었음
- 이러한 간소화된 non-local block은 squeeze-and-excitation(SE) block과 유사한 구조를 공유하고 있음
> - 두 방법 모두 aggregated features를 통해 original features를 업데이트함
- 결과적으로, 본 논문은 NL block과 SE block을 일반화시키고 통합한 global context(GC) block을 제안함
- GC block은 세 개의 module로 구성됨
> - (i) global context feature를 추출하기 위해 모든 pixel positions의 features를 aggregate하는 context modeling module
> - (ii) channel 별 상호의존성을 모델링하기 위한 feature transform module
> - (iii) global context feature를 모든 pixel positions의 features에 결합시키는 fusion module

## 접근법
- 시각화 결과로부터, non-local operation에서 계산되는 query-specific attention weights가 실제로는 query-independent하다는 것을 확인할 수 있었음
- 따라서, 모든 pixel positions에서 공유되는 query-independent한 global attention map을 계산하는 방식으로 non-local block을 간소화함
> - 입력 feature map에 대해 1x1 convolution 및 softmax function을 적용하여 global attention weights를 계산함 (context modeling)
> - 계산된 attention weights를 original feature map에 곱하여 scaling하고, scaled된 feature map에 다시 1x1 convolution을 적용하여 transform함 (transform)
> - 최종적으로, transformed된 feature map을 original feature map과 더하여 출력함 (fusion)
> - 즉, simplified non-local(SNE) block은 global context를 직접적으로 계산하고 이를 모든 pixel positions에 반영함
- SNE block은 SE block과 주조적으로 유사함
> - SE block은 GAP를 통해 global context modeling을 수행하며(squeeze operation), 2-layer MLP를 통해 bottleneck transform을 진행하고(excitation operation), 최종적으로 channel recalibration을 수행함 
> - NE block과의 큰 차이점은 SE block은 매우 가볍다는 것
- GC block은 SE block의 가벼운 연산과 SNL block의 효과적인 long-range dependencies 모델링을 결합하는 방식으로 구현됨
- SNL block에서 transform을 수행하는 1x1 convolution은 총 $C\times{C}$개의 많은 파라미터를 지니게 됨
- 이때 SE block에서 영감을 받아, 1x1 convolution을 bottleneck transform module로 교체함
> - 파라미터 수는 $2\times{C}\times{C}/r$개로 감소되며 $r$은 bottleneck ratio를 의미함
- ResNet의 여러 layers에 GC block을 추가한 GC-ResNet(GC-Net)을 설계함

## 실험결과
- Object detection, segmentation, classification에서 모두 좋은 성능을 기록함

## 의견
- / 