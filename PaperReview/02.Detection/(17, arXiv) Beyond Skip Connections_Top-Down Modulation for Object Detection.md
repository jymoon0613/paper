# Beyond Skip Connections: Top-Down Modulation for Object Detection (arXiv, 2017)

[논문링크](https://arxiv.org/abs/1612.06851)

<p align="center">
    <img width="800" alt='fig1' src="../img/shrivastava2016beyond.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Small-sized objects를 잘 detect하기 위해서는 fine-grained한 details가 요구되지만, CNN은 stage가 진행될수록 receptive field를 키워 context 정보에 집중하도록 설계되어 있음 (resolution이 감소함)
- 본 논문에서는 fine-grained details를 deep semantic context와 효과적으로 결합시키기 위한 top-down modulation process을 제안함 (TDM)
> - CNN의 bottom-up feedforward 연산 이후, 최종 high-level semantic features가 top-down network를 통해 하위 layers로 다시 전달됨
> - 전달된 high-level features는 해당 layer의 bottom-up intermediate features와 결합되며, 결합된 features가 다시 하위 layers로 전달되는 연속적인 구조임
> - 각각의 전달 과정에서 생성되는 features는 local/larger receptive fields를 모두 포함할 수 있음

## 접근법
- TDM은 lateral module과 top-down module로 구성됨
- Lateral module은 feature 결합을 위해 bottom-up features를 refine하는 역할을 함
> - Refined bottom-up features와 top-down features가 궁극적으로 결합됨
- Top-down module은 상위 layer의 top-down features를 upsample하여 하위 layers로 전달하는 역할을 함
- Faster R-CNN의 backbone으로 TDM이 추가된 네트워크를 사용함
> - Top-down pathway를 거친 마지막 feature maps가 RPN, detection에 사용됨

## 실험결과
- Vanilla Faster R-CNN에 TDM이 추가된 VGG16/ResNet101/InceptionResNetv2를 backbone으로 사용하는 경우, 기존의 pure backbone을 사용하는 것보다 성능이 유의미하게 개선되었음

## 의견
- / 