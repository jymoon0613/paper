# Knowledge Distillation: A Good Teacher is Patient and Consistent (CVPR, 2022)

[논문링크](https://openaccess.thecvf.com/content/CVPR2022/html/Beyer_Knowledge_Distillation_A_Good_Teacher_Is_Patient_and_Consistent_CVPR_2022_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/beyer2022knowledge.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 최근의 large-scale vision models은 여러 tasks에서 좋은 성능을 기록하고 있지만 computational cost가 매우 커서 real-world applications에 적용하기 매우 힘듦
- 본 논문에서는 knowledge distillation을 통해 성능이 높은 거대한 모델을 최대한 성능을 유지하면서 더 작은 크기의 효율적인 모델로 압축하는 방법에 대해 논의함
- 특히, 본 논문에서는 knowledge distillation을 teacher와 student에 의해 구현된 functions을 매칭시키는 작업으로 생각함

## 접근법
- 제안하는 방법의 효과를 검증하기 위해 클래스의 개수, 이미지의 수가 모두 상이한 여러 classification 데이터셋 사용
- Teacher 모델로는 사전 훈련된 BiT 모델 사용 (BiT-ResNet-152x2)
- Teacher와 student의 logits을 기반으로 KL-diverfence loss를 사용 (일반적인 KD loss만 사용)
- 성능 향상을 위해서는 consistent한 teaching이 중요함: teacher와 student가 서로 같은 view(crop, augmentations)를 처리해아 함
- 또한, generalization을 잘하기 위해 많은 양의 support points를 매칭할 수 있는 function을 찾아야 함
> - Teacher와 student의 prediction을 많이 매칭하는 함수를 찾아야 함
> - 즉, 매칭되는 예측 분포가 넓어야 함
> - 이를 위해 mixup과 같은 공격적인 augmentation 전략을 사용함
- 또한, 매우 공격적인 augmentation 전략을 통해 매칭되는 예측 분포가 넓어지므로 최적화를 위해 더 많은 epoch이 필요함 (patient teaching)
- 더 빠른 수렴을 위해 Shampoo optimizer를 사용함

## 실험결과
- 서로 다른 model family 간에도 잘 작동 (e.g. BiT-ResNet-152x2 to MobileNet v3)
- 서로 다른 image resolution을 갖는 teachers를 ensemble하여 사용하였을 때 성능이 더욱 증가함 (ImageNet SOTA 달성)

## 의견
- /