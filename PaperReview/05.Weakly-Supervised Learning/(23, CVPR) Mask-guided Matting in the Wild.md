# Mask-guided Matting in the Wild (CVPR, 2023)

[논문링크](https://openaccess.thecvf.com/content/CVPR2023/html/Park_Mask-Guided_Matting_in_the_Wild_CVPR_2023_paper.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/park2023mask.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Image matting은 foreground object와 surrounding backgrounds를 정확하게 분리하는 task를 의미함
- 기존의 image matting 기법은 주로 trimap을 사용
> - Trimap: 이미지를 pixel-level 수준에서 foreground, background, unknown 영역으로 나눈 map
- 하지만, trimap 생성을 위한 pixel-level annotation은 시간과 비용이 매우 크며, 따라서 practical한 어플리케이션 적용에 한계가 있음
- 최근 trimap 대신 훨씬 coarse한 mask를 guidance로 사용하는 mask-guided 기법이 관심을 받고 있음
> - Mask는 간단한 수작업으로 만들거나 pre-trained segmentation models로부터 획득 가능
- 매우 mask-guided 기법은 coarse한 spatial prior을 사용함에도 불구하고 trimap 기반의 기법과 매우 유사하거나 더 좋은 성능을 보여줌
- 하지만 현재의 mask-guided 기법은 복잡한 real-world scenes에 대한 image matting 성능은 낮다는 문제가 있음
- 본 논문에서는 복잡한 real-world 환경에서도 잘 generalize될 수 있는 mask-guided matting model의 학습 프레임워크를 제안함

## 접근법
- 제안하는 방법은 훈련 과정에서 세 가지 타입의 데이터셋을 사용함
> - Matting dataset: 입력 이미지와 그에 대한 정확한 matte 정보를 포함하고 있는 데이터셋
> - Background dataset: 훈련 샘플 생성 시 foreground objects가 합성되는 배경에 대한 데이터셋
> - Weak-supervision dataset: matting model을 real-world 환경으로 generalize하기 위한 데이터셋
- Image matting의 정확도를 높이기 위한 instance-wise learning을 수행
> - Matting dataset으로부터 입력 이미지 두 개와 이에 대한 matte 정보 두 개를 샘플링
> - Background dataset으로부터 배경 이미지를 하나 샘플링
> - 배경 이미지와 입력 이미지 두 개를 동시에 합성하고, matte 정보 두개도 합성함
> - 이때 matting model은 두 개의 입력 이미지 인덱스 중 하나를 랜덤하게 선택하고, 합성된 이미지에서 선택된 인덱스에 해당하는 image의 object matte를 예측하도록 훈련됨
- Matting model의 generalizability를 향상시키기 위해 weak-supervision 기반의 self-training 수행
> - Natural images에 대해 pre-trained instance segmentation 모델이 생성한 instance masks를 guidance로 사용
> - EMA 기반의 teacher-student 구조 사용
> - Teacher matting model은 weak-augmentation이 적용된 입력 이미지와 instance mask를 입력으로 받아 pseudo label을 생성
> - Student matting model은 strong-augmentation이 적용된 입력 이미지와 instance mask를 입력으로 받아 예측을 수행하고 teacher의 pseudo label에 대해 supervision 수행

## 실험결과
- 베이스라인인 MGMatting과 비교했을 때 image matting 퀄리티가 더 좋았음
- 특히 제안하는 방법은 다중 object가 있거나, 복잡한 배경이 있거나, 여러 categories의 object가 존재하는 real-world scenes에 대한 image matting 성능이 뛰어나 generalization이 잘 되었음

## 의견
- /