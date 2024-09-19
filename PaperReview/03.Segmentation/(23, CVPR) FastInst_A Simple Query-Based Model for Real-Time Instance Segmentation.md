# FastInst: A Simple Query-Based Model for Real-Time Instance Segmentation (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/He_FastInst_A_Simple_Query-Based_Model_for_Real-Time_Instance_Segmentation_CVPR_2023_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/he2023fastinst.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Instance segmentation은 image에 존재하는 모든 objects를 segment하는 것을 목적으로 함
> - Semantic segmentation과의 차이는 같은 class의 objects에 대해서도 서로 구분지을 수 있다는 것
- Instance segmentation을 위한 방법으로는 Mask R-CNN과 같이 detection과 segmentation을 순차적으로 수행하는 2-stage 방법이 있음
> - 매우 많은 region proposals를 생성하므로 computations이 많아짐
- 효율성을 개선하기 위해 FCN을 기반으로한 1-stage 방법도 제안되었음
> - 이러한 방법들은 region proposals 없이 object를 end-to-end로 segment하여 속도가 매우 빠르지만, NMS와 같은 manually-designes post-processing이 필요하다는 단점이 있음
- 최근에는 DETR의 도입과 함께 query 기반의 single-stage instance segmentation 방법들이 많이 제안되고 있음
- Mask2Former는 backbone에 pixel decoder와 masked attention Transformer decoder를 추가함으로써 instance segmentation flows를 간소화함
> - 더 이상 hand-crafted components를 요구하지 않음
- 하지만, Mask2Former는 많은 decoder layer와 무거운 decoder 구조로 인해 연산량이 매우 많으며 따라서 real-time으로는 부적합함
- 따라서, 본 논문에서는 정확하고 효과적인 real-time instance segmentation 프레임워크를 제안함 (FastInst)
> - Mask2Former에 세 가지 techniques를 적용함
> - (1) Transformer decoder의 initial queries로 feature map 상에서 높은 semantics를 가지는 pixel embedding을 선택하는 instance activation-guided queries를 사용함
> - (2) Query features와 pixel features가 번갈아가면서 업데이트되는 dual-path architecture를 사용함
> - (3) Masked attention이 suboptimal query update process에 빠지는 것을 방지하기 위해 ground truth mask-guided learning을 사용함

## 접근법
- FastInst는 backbone, pixel decoder, Transformer decoder로 구성됨
- 먼저, 입력 이미지로부터 서로 다른 resolution의 feature maps을 각각 추출함 ($C_3, C_4, C_5$)
> - 1x1 conv.를 통해 feature channel을 256으로 통일시킨 뒤 pixel decoder로 전달
- Pixel decoder는 contextual information을 aggregate하고 enhanced multi-scale feature maps를 출력함 ($E_3, E_4, E_5$)
> - 가벼운 pixel decoder를 사용함 
> - 기존의 vanilla FPN에서 pyramid pooling module이 적용된 PPM-FPN으로 변경
- 중간 크기의 enhanced feature map($E_4$)에서 instance activation-guided queries(IA-guided queries)를 선택함
> - 이때 IA-guided queries를 구하기 위해 $E_4$에 각 픽셀마다의 class probability prediction을 수행하는 auxiliary classification head를 연결함
> - Auxiliary classification head의 output $P$에 argmax를 적용하여 각 픽셀별 foreground probability를 얻음
> - $E_4$에서 high foreground probabilities를 갖는 pixel embeddings을 object queries로 선정함
> - 먼저, 특정 region 내의 pixel 중 가장 높은 foreground probabilities를 갖는 pixels를 선별하고, 그 안에서 다시 foreground probabilities를 기준으로 object queries를 선정함
> - Auxiliary classification head는 matching-based Hungarian loss를 사용하여 supervised하게 훈련됨
- IA-guided queries를 auxiliary learnable queries와 concat하여 total queries를 구성함
> - Auxiliary learnable queries는 background pixel features를 그룹화하고 general한 image-independent information을 모델링하기 위해 사용됨
- Transformer decoder는 total queries와 가장 큰 크기의 enhanced feature map(pixel features, $E_5$)을 입력으로 받음
- Transformer decoder에서 pixel features와 total queries는 번갈아가면서 업데이트되며, 각 decoder layer에서 object class와 segmentation mask를 예측함
> - 한 Transformer decoder layer에서 한번의 pixel feature update와 한번의 query update가 모두 진행됨
> - Dual-path update strategy를 통해 더 fine-grained한 feature embeddings를 얻을 수 있으며, 증가되는 표현력은 pixel decoder에 대한 의존성을 완화함
- Pixel feature update는 pixel features를 query로, total queries를 key, value로 하는 cross-attention을 통해 수행됨
- Query update는 masked attention과 self-attention을 연속적으로 적용하여 수행됨
> - Total queries가 query로, update된 pixel features가 key, value로 사용됨
> - Masked attention은 query가 이전 decoder layer에서 예측된 mask 내의 foreground region에 대해서만 attend하도록 함
> - Self-attention을 통해 context information을 모델링함
- 3-layer MLP 두 개를 refined IA-guided queries에 부착하여 object classes와 mask embeddings를 예측하도록 함 (각 decoder layer에 모두 적용, 공유 X)
- Refined pixel features에는 linear projection을 부착하여 mask features를 계산함
- Mask embeddings는 mask features와 곱해져서 각 쿼리에 대한 segmentation masks가 생성됨
- Transformer decoder의 masked attention은 각 query의 receptive field를 제한할 수 있고, 이는 suboptimal한 query update process를 유발할 수 있음
- 따라서, 이를 해결하기 위해 ground-truth mask-guided learning을 사용함
> - 기존에 각 layer에서 masked attention을 위해 사용되던 predicted mask를 ground-truth mask로 대체함
> - Ground-truth mask와 매칭되지 않는 object queries의 경우 기존의 masked attention을 적용함

## 실험결과
- COCO dataset에서 평가한 결과 대부분의 이전 SOTA real-time instance segmentation algorithms를 성능, 속도 측면에서 개선함

## 의견
- /