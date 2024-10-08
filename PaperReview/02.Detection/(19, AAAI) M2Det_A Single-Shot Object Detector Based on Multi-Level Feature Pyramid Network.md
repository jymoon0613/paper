# M2Det: A Single-Shot Object Detector Based on Multi-Level Feature Pyramid Network (AAAI, 2019)

[논문링크](https://ojs.aaai.org/index.php/AAAI/article/view/4962)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhao2019m2det.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Feature pyramids는 object instances의 scale variation으로 인해 발생하는 문제를 완화함으로써 detection 성능을 크게 끌어올리는 역할을 함
- 하지만 현재의 feature pyramid는 (1) classification task를 위해 설계된 backbone에 기반하여 feature map을 추출하므로 object detection을 위한 충분한 representativeness가 부족하고, (2) feature pyramids의 각 level feature map은 주로 backbone의 single-level information으로 구성되어 있으므로 객체의 외형에 따른 인식 성능의 차이가 존재함
> - High-level features는 classification에 적합하고 더 복잡한 외형을 표현할 수 있음
> - Low-level features는 localization에 더 적합하고 단순한 외형을 표현하는 데 적합함
> - 실제 환경에서 object instances는 서로 다른 복잡도의 외형으로 존재하므로, single-level feature map을 사용하는 경우 일부 객체만이 포착될 수 있음 (suboptimal detection performance)
- 따라서, 본 논문에서는 앞서 언급한 단점들을 개선하면서 서로 다른 scale의 objects를 잘 detect할 수 있는 보다 효과적인 feature pyramid를 디자인하고자 함 (Multi-Level Feature Pyramid Network, MLFPN)
- MLFPN의 성능을 검증하기 위해 SSD 구조에 MLFPN을 결합한 간단하면서 강력한 one-stage detector인 M2Det을 제안함

## 접근법
- MLFPN은 세 가지 모듈로 구성됨: Feature Fusion Module (FFM), Thinned U-shape Module (TUM), Scale-wise Feature Aggregation Module (SFAM)
- VGG-Net을 backbone으로 사용하고, FFMv1은 backbone의 shallow 및 deep feature map을 fuse하여 base feature map을 생성함 (multi-level semantic information을 제공)
- Base feature map을 바탕으로 TUM은 서로 다른 크기의 feature maps을 생성함
- 여러 TUM을 stack함
- TUM은 U-shaped encoder-decoder 구조로 이루어짐
- 이때 FFMv2는 가장 큰 scale의 TUM output으로부터 base feature map과의 fuse를 통해 작은 scale의 feature map 생성 과정(TUM을 통해)에 관여함
- SFAM은 stacked TUM에서 output된 multi-level multi-scale의 feature map을 같은 scale 별로 concatenation하고, channel-wise attention(SE block)을 적용함
- 최종적인 여러 scale의 feature maps는 detection을 위해 사용됨

## 실험결과
- MS COCO 벤치마크에서 one-stage detector의 SOTA 성능을 기록함
- Mutli-level feature는 객체의 외형(appearance)이 복잡한 상황을 처리하는데 유용하게 사용됨

## 의견
- Multi-scale에 대한 고려뿐만 아니라 multi-level에 대한 고려를 추가함