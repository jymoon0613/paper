# Scale-Transferrable Object Detection (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhou_Scale-Transferrable_Object_Detection_CVPR_2018_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/zhou2018scale.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Scale problem은 object detection의 핵심적인 문제점 중 하나임
> - 서로 다른 scale로 resize된 입력 이미지를 모두 모델로 입력하여 여러 scale의 feature maps를 얻어 detection을 수행하는 image pyramid 방식은 연산량과 메모리 소모량이 매우 크며, real-time performance를 기대하기 어렵다는 문제가 있었음
> - CNN을 기반으로 low/high-level features가 abstract된 single feature map에서 서로 다른 scale, aspect ratios의 objects를 예측하는 방법이 제안되었지만 single feature map에서만 detection을 수행하는 것은 서로 다른 scale로 존재하는 objects를 detect하기 어렵다는 문제가 있음 (i.e., Faster R-CNN)
> - CNN의 서로 다른 layer의 feature maps에서 모두 detection을 수행하는 pyramidal feature hierarchy가 제안되었지만, shallow layer의 features는 semantic 정보가 부족하기 때문에 detection 성능이 크게 떨어진다는 문제가 있음 (i.e., SSD, MS-CNN)
> - Top-down 방식으로 high-level semantic feature maps를 low-level feature maps와 결합해나가는 feature pyramid network가 제안되어 모든 level의 feature maps에서 충분한 semantic 정보를 함유할 수 있도록 했지만, 성능이 feature pyramid의 디자인에 민감하며 추가적인 연산량이 발생한다는 단점이 있었음 (i.e., FPN)
- 본 논문에서는 FPN과 유사하게 high-level semantic 정보를 충분히 포함하고 있는 multi-scale feature maps을 생성하면서, FPN보다 추가적인 연산량이 더 적어 detector의 speed 손실이 적은 scale-transfer module(STM)을 제안함
> - DenseNet의 densely connected network structure를 활용함
> - STM은 small scale feature maps을 얻기 위한 pooling layer와 large scale feature maps를 얻기 위한 scale-transfer layer로 구성됨
- 최종적으로 STM을 사용하는 one-stage detector인 scale-transferrable detection network (STDN)를 제안함

## 접근법
- DenseNet-169를 backbone으로 사용함
- Backbone으로부터 서로 다른 scale의 feature maps를 획득하기 위해 scale-transfer module을 사용함
> - Low-resolution feature maps을 얻기 위해 mean pooling layer를 사용함
> - High-resolution feature maps을 얻기 위해 scale-transfer layer를 사용함
- Scale-transfer layer는 feature channels의 일부를 flatten하여 feature map의 width와 height를 확장하는 데 사용함
> - 이는 추가 파라미터를 사용하지 않으면서, channel 수는 줄이고, resolution은 키우는 목적을 달성할 수 있음
- Scale-transfer module을 통해 총 6개의 서로 다른 scale의 feature maps를 획득함
> - DenseNet의 densely connected network structure로 인해 각 layer의 feature maps는 이미 충분한 low-level detail과 high-level semantic을 포함하고 있으므로 정보 combine을 위한 별도의 feature pyramid가 필요 없음
- 생성된 6개의 feature maps 상에서 detection을 수행함

## 실험결과
- STDN은 SSD와 비슷한 속도로 동작하면서 성능은 더 좋았고, SSD의 개선된 버전인 DSSD보다는 성능이 약간 낮았지만 속도면에서는 훨씬 뛰어났음 (COCO, PASCAL VOC)

## 의견
- Intro와 related work에 multi-scale object detection 관련하여 잘 정리되어 있음 