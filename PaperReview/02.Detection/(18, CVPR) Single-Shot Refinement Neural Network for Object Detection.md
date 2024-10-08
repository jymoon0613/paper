# Single-Shot Refinement Neural Network for Object Detection (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhang_Single-Shot_Refinement_Neural_CVPR_2018_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/zhang2018single.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Two-stage 방식의 object detector는 뛰어난 성능을 보여줌
- Two-stage 방식은 one-stage 방식보다 다음과 같은 세 가지 이점이 있음
> - Two-stage 구조와 함께 sampling heuristic을 사용하여 class imbalance 문제가 덜 심각함
> - 예측된 bounding box의 파라미터를 최적화하기 위해 two-stage cascade 사용
> - Object를 표현하기 위해 two-stage features를 사용함 (object 존재 여부 -> 어떤 object인지)
- 본 논문에서는 앞서 언급한 two-stage detector의 장점을 모두 살릴 수 있는 one-stage detector를 제안하고자 함 (RefineDet)

## 접근법
- RefineDet는 상호 연결된 두 개의 module인 anchor refinement module(ARM)과 object detection module(ODM)으로 구성됨
- ARM은 negative anchors의 수를 줄여서 classifier의 search space를 축소시켜주며 anchors의 location과 size를 일정 수준 조절함
- ARM은 backbone 네트워크의 classifier를 제거하고, 일부 layer에 convolution layer를 추가한 구조
- ARM의 결과로 설정된 각 레이어마다 조정된 anchor box의 정보를 담고 있는 feature map과 object/background label에 대한 정보를 담고 있는 feature map 총 두 개를 얻을 수 있음
- ODM은 refined anchors로부터 정확한 object location 및 multi-class labels를 예측함 (몇 개의 convolution layer로 구성)
- RefineDet의 핵심은 다음과 같음
> - ARM의 features를 효과적으로 ODM에 전달하기 위한 transfer connection block(TCB)
> - Objects의 location과 size를 정확하게 regress하는 two-step cascade regression
> - Easy negative anchors를 사전에 제거하여 class imbalance 문제를 완화시켜주는 negative anchor filtering
- One-stage detector는 작은 object를 포착하지 못한다는 문제가 있음
- 이를 위해 ARM이 anchors를 미리 refine하고 ODM이 detection을 수행하는 two-step cascade regression 방법을 사용하여 세밀한 예측을 수행함
- TCB(transfer connection block)는 ARM과 ODM을 연결시키기 위해서 ARM의 서로 다른 layer로부터 비롯된 feature를 ODM이 요구하는 형태에 맞게 변환시켜주는 역할
- ARM이 출력한 refined anchor box에 대하여 만약 negative confidence가 사전에 지정한 threshold보다 높을 경우 ODM에 해당 anchor box를 전달하지 않음 (negative anchor filtering)

## 실험결과
- Pascal VOC 2007, Pascal VOC 2012, MS COCO을 사용한 general object detection task에서 SOTA 성능 달성
- ARM이 마치 RPN의 역할을 수행하여 anchor box의 위치와 크기를 보다 정교하게 예측
- TCB가 Feature Pyramid Network와 유사한 기능을 수행하여 high-level feature map의 정보를 활용하여 성능을 향상

## 의견
- /