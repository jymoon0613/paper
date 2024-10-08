# Spatiotemporal Contrastive Video Representation Learning (CVPR, 2021)

[논문 링크](https://openaccess.thecvf.com/content/CVPR2021/html/Qian_Spatiotemporal_Contrastive_Video_Representation_Learning_CVPR_2021_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/qian2021spatiotemporal.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 이미지에서의 Self-supervised Learning은 이미지의 공간적 특성 (Spatial Information)에 집중 
- 비디오에서의 Self-supervised Learning은 비디오의 시간적 특성 (Temporal Information)에 집중 
- 비디오에서의 Self-supervised Learning은 공간적 특성을 충분히 활용하지 못하고 있음 
- 따라서 본 논문에서는 Contrastive Video Representation Learning (CVRL) 프레임워크를 제안하여 비디오에서의 시공간적 특성 (Spatiotemporal Information)을 동시에 학습하는 것을 목표로 함

## 접근법
### (1) InfoNCE Contrastive Loss 
- CVRL을 훈련하기 위한 Loss Function 정의 
- 추출된 Encoded Representation의 유사도를 기반으로 정의 
### (2) Video Encoder 
- 3D-ResNets을 Backbone Networks로 사용하여 비디오를 인코딩 
- 기존의 모델을 조금 수정 
- MLP를 제거하고 2048 차원의 Representation을 그대로 인코딩 벡터로 사용하여 해당 비디오 인코더가 다른 지도학습 방법에서도 사용할 수 있도록 함 
### (3) Data Augmentation 
- Spatiotemporal Information 학습을 위해 Data Augmentation 방법을 고안하고 Self-supervised Learning 진행 
- Temporal Augmentation : 인코더가 Temporal Information을 효과적으로 학습할 수 있도록 확률분포를 이용한 샘플링 기법을 사용. Augmentation 되는 비디오 프레임이 너무 멀리 떨어지지 않도록 함. 너무 멀리 떨어지는 경우 인코더는 Temporal Information이 아닌 Temporally Invariant Features를 학습하게 됨. 
- Spatial Augmentation : 기존 Image에 적용하는 Augmentation 기법들 (Cropping, Blurring,…)을 비디오의 각 프레임에 바로 적용할 경우, 이러한 순차적 프레임에 나타나는 무작위성은 Temporal Dimension에 따른 Representation 학습을 방해할 수 있음. 따라서, 비디오의 Temporal Dimension을 따라 일관된 Augmentation을 적용하여 인코더가 Temporal Information을 보존하면서도 Spatial Information을 효과적으로 학습할 수 있도록 함. 

## 실험결과
- Action Classes를 담고 있는 Kinetics-400 및 Kinetics-600 데이터셋을 사용하여 평가 
- Representation 추출을 위한 인코더 네트워크를 제안하는 Self-supervised 방식으로 Pre-training함 
- 훈련된 인코더 네트워크의 가중치를 고정시키고, Classification을 위한 Linear Layer 하나를 추가한 뒤 Transfer Learning 진행하여 평가함 (Linear Evaluation Protocol)
- 데이터의 작은 부분집합을 통해 훈련된 인코더 네트워크를 Fine-tuning하여 Semi-supervised Learning을 진행하고 평가함 
- 훈련된 인코더 네트워크를 UCF-101, HMDB-51 데이터셋에 기반한 Action Classification 문제에 적용하고 평가함 (Linear Evaluation, Fine-tuning 각각 평가) (Downstream Task 1) 
- 훈련된 인코더 네트워크를 AVA 데이터셋을 기반한 Action Detection 문제에 적용하고 평가함 (Downstream Task 2)
- 훈련된 인코더 네트워크의 가중치를 고정시키는 것이 아닌, Classification을 위한 Linear Layer가 추가된 인코더 네트워크를 End-to-End로 Supervised Learning시키고 평가함 

## 의견
- 이미지 분야에서의 Spatial Information Representation과 비디오 분야에서의 Temporal Information Representation을 혼합한 방식으로 Representation Learning 진행 
- Self-supervised Learning, 그 중에서도 Contrast Learning 기법 사용 
- 인코더 네트워크를 Pre-training하고 여러 Downstream Task에 적용함 

