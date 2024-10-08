# Deformable Convolutional Networks (ICCV, 2017)

[논문링크](https://openaccess.thecvf.com/content_iccv_2017/html/Dai_Deformable_Convolutional_Networks_ICCV_2017_paper.html)

<p align="center">
    <img width="400" alt='fig1' src="../img/dai2017deformable.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Visual recognition task의 핵심은 obejct의 scale, pose, viewpoint, part deformation의 관점에서의 variation 및 transformation을 수용할 수 있는 강건한 모델을 구축하는 것
- 이러한 목적을 달성하기 위해 훈련 데이터셋에 augmentation 기법을 적용하여 훈련 데이터가 충분한 변형 형태를 포함하도록 하는 방법과 transformation-inv ariant한 피처(i.e. SIFT) 혹은 알고리즘(i.e. sliding window)을 이용하는 방법이 있음
- 하지만 위와 같은 방법들은 두 가지 단점이 존재함
> - 훈련 데이터셋에 변형을 주는 경우 설계자의 prior를 기반으로 고정된 augmentation method를 설계하게 되며 이는 unknown geometric transformaton을 수반하는 새로운 task에서 일반화되기 힘듦
> - 수작업을 설계된 transformation-invariant한 피처 및 알고리즘은 지나치게 복잡한 transformaton의 경우 대응이 힘들 수 있음
- 최근의 CNN은 여러 vision tasks에서 좋은 성능을 보여주지만 여전히 위의 두 단점을 내포하고 있음
> - CNN의 geometric transformations에 대응하는 능력은 방대한 양의 data augmentation, 큰 model capacity, 수작업으로 설계된 일부 모듈(i.e. max-pooling)에 기반함
- CNN은 크고 예측 불가능한 transformations에 대응하기 힘듦
- 이는 CNN 모듈의 고정된 geometric structures로부터 기인함 (geometric transformations을 처리하는 내부 메커니즘이 부족)
> - Convolution unit은 고정된 locations 내에서 input feature map을 샘플링함
> - Subsampling layer는 ROI를 고정된 spatial bins로 구분함
- 이러한 CNN의 단점은 다음과 같은 큰 문제를 야기함
> - 같은 CNN layer에 속하는 모든 activation units의 receptive field 사이즈가 동일함
> - 이는 spatial locations에 대한 의미론적 정보를 인코딩하는 high-level CNN layer의 경우 부적절함
> - 서로 다른 locations는 서로 다른 scales 및 deformation의 object와 대응될 수 있기 때문에 scales 혹은 receptive field 사이즈를 적응형으로 결정하는 것이 중요함
> - 이는 fine localization이 필요한 visual recognition (i.e. semantic segmentation, object detection)의 경우 더욱 중요함
- 따라서, 본 논문에서는 CNN의 geometric transformations 모델링 능력을 크게 향상시킬 수 있는 두 가지 새로운 모듈을 제안함
> - (1) Deformable convolution: 기존 convolution의 grid sampling locations에 2D offsets를 더해줌 (sampling grid의 자유로운 변형을 가능하게 함)
> - Offsets은 입력 feature map으로부터 추가적인 convolution layer를 적용하여 계산됨 (sampling grid의 변형은 local, dense, adaptive한 방식으로 입력 features에 따라 조정됨)
> - (2) Deformable ROI pooling: 이전의 ROI pooling의 bin partition 내의 각 bin position에 offset을 더해줌
> - 마찬가지로 offsets은 입력 feature map에 기반하여 적응형으로 결정됨
- 이러한 모듈들은 기존의 CNN 구조에 추가 파라미터가 거의 없이 쉽게 적용가능하며, 이러한 CNN 구조를 Deformable Convolutional Networks(DCN)이라고 칭함

## 접근법
- Deformable convolution에서는 기존의 고정된 sampling grid가 offsets을 통해 증강됨
- Deformable convolution은 offset을 구하는 branch와 일반 convolution을 구하는 branch로 구성됨
> - Offsets을 구하기 위해 input feature map에 convolutional을 적용
> - 이때의 convolution은 일반 convolution과 같은 kernel size를 적용
> - Offset convolution의 output feature map 크기는 input feature map의 size와 동일하며 각 spatial position에서의 값은 해당 position의 offset값을 의미함
> - Offset은 x,y 축에 대한 값을 표현해야 하므로 offset convolution의 output channel의 수는 2배가 됨
> - Offset 값은 실수가 될 수 있으며 interpolation을 통해 mapping이 가능함
> - Offset mapping이 적용된 feature map에 기존의 convolution을 수행
> - 두 branch는 backpropagation을 통해 동시에 학습 가능함
- Deformable ROI pooling도 deformable convolution과 거의 유사하게 정의됨
> - Offset을 구하는 branch와 ROI pooling을 수행하는 branch로 구성
> - 입력 feature map에 ROI pooling을 수행하고, output feature map을 FC layer로 입력하여 normalized된 offsets을 얻음
> - 이후 normalized offsets을 ROI의 width와 height로 transformed 시키고 offsets의 magnitude를 modulate하는 pre-defined scalar를 곱하여 최종 offsets을 얻음
> - ROI size에 invariant한 offset을 학습하기 위해 offset normalization 과정이 필수적
- Deformable convolution 및 deformable pooling 모두 기존 연산과 동일한 input과 output을 지니기 때문에 기존 네트워크에 쉽게 적용될 수 있음
> - 기존의 detection 모델에 적용
> - Feature extraction을 수행하는 CNN backbone 및 detection head의 pooling 연산 일부를 deformable 버전으로 변경

### 실험결과
- Deformabel convolution 및 pooling을 사용했을 때 기존 베이스라인 성능을 능가
- 기존 convolution의 receptive field와 sampling grid는 깊은 레이어로 가능 과정에서 고정적이지만, deformable convolution은 object의 scale 및 shape에 따라 이를 적응적으로 조절할 수 있음

## 의견
- /