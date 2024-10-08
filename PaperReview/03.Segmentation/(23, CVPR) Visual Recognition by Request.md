# Visual Recognition by Request (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Tang_Visual_Recognition_by_Request_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/tang2023visual.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Visual recognition은 지난 10년간 딥러닝 모델 및 large-scale datasets과 함께 크게 발전했음
- 본 논문에서는 visual recognition의 비약적인 발전에도 불구하고 visual recognition의 'unlimited granularity'에 대해서는 그동안 거의 논의되지 않았다는 점을 지적함
> - 인간은 주어진 이미지로부터 어떠한 임의의 fine-grained contents라도 인식할 수 있는 반면 현재의 visual recognition system은 불가능함
- Visual recognition system이 fine-grained contents를 잘 인식하지 못하는 주된 이유는 fine-grained/long-tailed concepts의 경우 매우 적은 training data로 인한 제약이 존재하기 때문임
- 하지만, 저자들은 annotations의 제약보다 인식의 세분성(granuality)과 인식의 확실성(certainty) 간의 상충이 훨씬 더 근본적인 원인이라고 주장함
> - 즉, annotation이 더 세분화될수록 annotation의 certainty는 필연적으로 낮아짐
- 이에 저자들은 visual recognition의 세분성이 instances와 scenarios에 따라 가변적이어야 한다고 주장함
- 따라서, 본 논문은 semantic annotations를 requests라는 훨씬 더 작은 단위로 취급하고 해당 requests가 요구되었을 때만 recognition이 수행된다고 가정함
> - Recognition의 granuality를 object size, importance, clearness 등에 따라 자유롭게 조정할 수 있음
- 결과적으로, 본 논문은 새로운 패러다임인 visual recognition by request(ViReq)를 제안함
> - Visual recognition은 훨씬 작은 단위인 requests로 분해함
> - Requests는 두 가지 타입으로 나누어짐: (i) instance를 semantic parts로 분해하거나, (ii) semantic region으로부터 instance를 segment함
> - 첫번째 타입(i)에 대해서는 knowledge base가 hierarchical text-based dictionary로서 기능하여 segmentation 과정을 가이드할 수 있음
- ViRReq는 이전의 visual recognition system과 비교하면 두 가지 장점이 있음
> - ViRReq는 기존의 methods와는 달리 매우 불완전한 annotations로부터 복잡한 visual concepts를 학습할 수 있음
> - ViRReq는 knowledge base를 업데이트하고 매우 적은 training examples를 annotate하는 것만으로도 새로운 visual concept를 쉽게 추가할 수 있음
- ViRReq를 구현하기 위해 query-based recognition algorithm을 설계함
> - (i) 이미지로부터 visual features를 추출하고, (ii) request와 knowledge base로부터 text embedding vectors를 계산하고, (iii) 이미지와 text features의 interaction을 모델링함

## 접근법
- 기존의 image segmentation은 주어진 입력 이미지($X$)의 각 픽셀에 적절한 class label을 할당함($Z$)
- ViRReq는 이러한 task를 일련의 requests($\mathcal{R}$)로 분할함
> - $\mathcal{R}$은 request $\mathcal{Q}$와 answer $\mathcal{A}$로 구성됨
> - Requests는 순차적으로 수행됨
> - Segmentation 결과는 tree 구조로 저장됨
- Requests는 두 가지 타입으로 구분됨
> - Whole-to-part semantic segmentation: instance를 semantic parts로 분해 (Type-I)
> - Instance segmentation: semantic region으로부터 instance를 segment함 (Type-II)
- 첫 단계에서 전체 이미지는 special instance인 SCENE으로 처리되며, sky, building, person 등의 top-level classes가 parts로 고려됨
- Type-I과 Type-II requests를 반복적으로 처리함으로써 어떠한 임의의 fine-grained units도 segment할 수 있음
- Type-I requests의 whole-part hierarchy를 구현하기 위해 knowledge base를 이용함

## 실험결과
- CPP 및 ADE20K dataset에서 검증함
- Segmentation accuracy를 평가하기 위한 지표인 hierarchical panoptic quality(HPQ)를 제안함
- Open-domain recognition에서도 좋은 성능 기록

## 의견
- / 