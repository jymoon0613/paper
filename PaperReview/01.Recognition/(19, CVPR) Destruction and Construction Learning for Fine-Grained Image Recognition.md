# Destruction and Construction Learning for Fine-Grained Image Recognition (CVPR, 2019)

[논문 링크](https://openaccess.thecvf.com/content_CVPR_2019/html/Chen_Destruction_and_Construction_Learning_for_Fine-Grained_Image_Recognition_CVPR_2019_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/chen2019destruction.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Fine-grained Image Classification은 얼핏 보면 비슷한 Object가 포함된 이미지에서 국소 영역의 세부 사항을 식별하여 각각을 분류하는 작업을 의미한다. 
- 이제까지의 Fine-grained Image Classification을 위한 방법론은 (1) 이미지에서 식별의 대상이 되는 Object Parts를 특정한 후 그로부터 분류를 수행하는 방식과, (2) 비지도 방식의 Attention Mechanism을 사용하여 Object Parts를 자동으로 찾아내는 방식으로 구분 가능하다. (1)은 Object Parts를 특정하는 Bounding Box Annotations이 필요하다는 점에서 불편함이 있으며, (2)는 Attention Mechanism을 위한 추가적인 Module이 필요하므로 Computation Overhead가 발생한다. 
- 본 연구에서는 Deconstruction and Construction (DCL)이라는 새로운 프레임워크를 제안하여 Object Parts를 자동으로 특정하여 각 지역 영역 간의 의미론적 상관 관계를 모델링하여 분류 성능을 끌어올리는 방법을 제안한다. DCL은 Attention Mechanism과 유사하게 자동으로 Object Parts를 추출한다는 점에서 (2)와 유사하지만, 이는 훈련 단계에서만 적용되므로 Inference 시에는 Computation Overhead가 발생하지 않는다. 

## 접근법
### (1)  Destruction Learning 
- NLP에서와 유사하게 이미지를 여러 국소 영역으로 나누고 섞는 것은 네트워크가 이미지의 세부적인 특징들에 집중하여 분류하도록 강제할 수 있음 
- 제안된 Region Confusion Mechanism (RCM)은 이미지를 여러 영역으로 나누고 섞음 
- Classification Network는 Global Structure가 무너진 이미지를 통해 정확한 분류를 수행해야 하므로 좀 더 지역적인 피처들에 집중해야 함 
- 하지만 RCM을 통해 이미지 패치를 섞을 때 의도치 않게 Noisy한 시각적 패턴이 생길 수 있고 이는 학습 과정에 악영향을 미침 
- 이를 위해, Adversarial Loss를 도입하여 노이즈 패턴이 Feature Space에 과도하게 들어가는 것을 방지함 
### (2)  Construction Learning 
- 이미지가 여러 영역의 복잡하고 다양한 시각적 패턴의 조합으로 이루어져 있다는 점을 고려해보았을 때, 지역 영역 간의 상관 관계를 모델링하는 학습 방식을 생각해 볼 수 있음 
- 즉, Destruction된 이미지에서 각 패치들의 원래 위치를 예측하도록 학습하는 Region Alignment Network를 도입하여 지역 영역 간의 상관 관계를 모델이 추가적으로 학습할 수 있도록 함 

## 실험결과
- CUB, Stanford Cars, FGVC-Aircraft와 같은 Fine-grained Classification Data에 대해 학습함 

## 의견
- 이번 연구에서 추구하고자 하는 방향과 유사함 
- Self-supervised 요소가 다수 들어가 있지만 엄밀히 말하면 SSL은 아님 (Destruction은 이미지 전처리 과정에서 수행되며 Construction 혹은 Adversarial 과정은 Labeled 데이터를 통한 Supervised 방식으로 동시에 진행됨) 
- 훨씬 Self-supervised Learning에 가깝게 변경? 
- 더욱 성능을 개선시킬 방법은? 