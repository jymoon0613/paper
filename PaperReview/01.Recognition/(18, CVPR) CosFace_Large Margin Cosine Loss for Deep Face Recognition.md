# CosFace: Large Margin Cosine Loss for Deep Face Recognition (CVPR, 2018)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Wang_CosFace_Large_Margin_CVPR_2018_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/wang2018cosface.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Face recognition(FR)은 크게 face verification과 face identification의 두 sub-tasks로 나누어지고, 두 tasks는 face detection, face extraction, classification의 세 단계로 구성됨
- 하지만, classifier의 softmax function은 classification을 위한 충분한 discriminative power를 제공하지 못한다는 문제가 지적되어왔음
- 본 논문에서는 기존의 softmax loss를 cosine loss로 reformulate함 (Large Margin Cosine Loss, LMCL)
> - Inter-class cosine margin을 최대화하여 discriminative features를 학습
- 결론적을 LMCL로 학습하는 deep FR 모델을 도출함 (CosFace)

## 접근법
- Softmax function에서 features와 weights을 L2 normalize해주게 되면 class probability는 weight와 features 사이의 각도에만(cosine of angle) 의존하여 결정됨
> - Normalized version of Softmax Loss(NSL)
> - 모델은 angular space에서 separable한 features를 학습함
> - 1번 class에 대해 $cos(\theta_1) > cos(\theta_2)$가 되도록 학습, 이때 $\theta$는 feature와 weight 사이의 각도
- 하지만 NSL만으로는 충분한 discriminative features를 학습하기에 부족하고, 본 논문에서는 classification boundary에 cosine margine을 추가함
> - 즉 $cos(\theta_1)-m > cos(\theta_2)$이 되도록 학습하며, $m$은 고정된 하이퍼파라미터
> - A-Softmax와 다른 점은, A-Softmax의 경우 각 class마다 decision boundary의 margin이 consistent하지 않지만 LMCL의 경우 $m$으로 consistent함

## 실험결과
- LFW, YTF, MegaFace에서 실험 진행
- 다른 loss functions를 사용했을 때보다 좋은 성능 기록
- LFW, YTF, MegaFace에서 SOTA 기록

## 의견
- /