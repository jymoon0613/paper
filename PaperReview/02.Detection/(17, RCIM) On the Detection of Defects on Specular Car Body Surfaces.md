# On the Detection of Defects on Specular Car Body Surfaces (RCIM, 2017)

[논문링크](https://www.sciencedirect.com/science/article/pii/S0736584517300194)

<p align="center">
    <img width="400" alt='fig1' src="../img/molina2017detection.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 제품 품질 관리 (product quality control)은 생산 라인에서 가장 중요한 프로세스 중 하나이며, 고객의 기대를 충족시키기 위해 여러 검사를 진행
> - 제조업체는 원판, 도장, 마감 등 각 조립 단계에서 품질 관리 라인을 보유
- 수동 검사 과정은 (1) 검사관마다 평가 기준이 달라 같은 수준의 품질 기준을 보장할 수 없다는 것과 (2) 미리 오류를 감지하고 대응하는 빅데이터 분석이 불가능하다는 문제가 있음
- 또한, 초기 단계의 불량은 눈으로 식별불가능할만큼 미세하며, 이러한 불량을 조기에 식별하지 못하면 이후 공정에서 불량이 점점 커지고 심각해질 수 있음
- Flat surfaces의 결함 식별에는 주로 line scan camera가 사용됨
> - Line scan camera: 스캐너의 작동방식과 유사하게 pixels를 line 단위로 수집하는 카메라 (area camera로는 커버되지 않는 넓은 범위의 pixels를 수집해야 하거나, area camera를 설치하기 힘든 경우 사용)
> - High-resolution line scan camera로 표면 정보를 수집
> - 입력 표면 정보와 정상적인 표면 정보를 frequency domain에서 비교하여 결함 여부를 판단
> - 이러한 방식은 오목한 표면이나 위치 변화가 있는 표면에서는 사용이 불가능
- 본 논문에서는 평평한 영역과 완만하게 경사가 변하는 영역뿐만 아니라 급격한 포면 경사 변화가 있는 영역에서도 결함을 감지할 수 있는 자동화 비전 검사 접근 방식을 제안함
> - Deflectometry(편향 측정) 및 image fusion(이미지 융합) 기술을 활용한 pre-processing, post processing을 제안함

## Preliminaries
- Deflectometry-based detection on specular surfaces: 반사면의 편향 즉정 기반 감지는 자체 표면의 결함을 감지하는 신뢰성 있는 검사 방식임
> - 수동으로 검사하는 경우 검사관이 차체에 반사되는 차체 반사광을 분석하고 변형을 식별
> - Deflectometry는 표면에 구조화된 light patterns를 projection하는 방식에 기반함
> - 반사면에 변형이 나타나면 light rays(광선)이 편향되어 카메라로 인식되는 패턴 모양이 급격하게 변화됨
- 반사면의 편향 즉정 기반 감지는 빛 반사와 관련된 몇 가지 문제점이 있음
> - 반사광은 specular spike(반사 스파이크), specular lobe(반사 로브), diffuse lobe(확산 로브)의 세 가지 구성 요소로 이루어져 있음
> - Diffuse lobe는 내부 반사의 결과이고, specular spike와 specular lobe는 표면 반사의 결과
> - 표면이 완벽하게 반사된다면 diffuse lobe가 관측되지 않지만 표면 특성이 완전 반사에서 비반사로 변경됨에 따라 specular spike가 감소하고 diffuse lobe가 더 강해짐
> - 표면 구조에 관한 정보는 주로 specular spike에 의해 포착되므로 diffuse lobe의 영향을 최소화하는 것이 중요함
- Image fusion은 multi-sensor, multi-temporal, multi-view의 상호보완적인 정보를 하나의 새로운 이미지로 통합하는 것
> - 본 논문에서는 서로 다른 시점에 단일 카메라로 획득한 여러 이미지를 병합하여 새로운 이미지를 형성하는 프로세스로 사용

## 접근법
- Pre-processing step에서는 scanning stage에서 수집된 images를 fusion image로 변환함
> - 프레임 간의 squared-distance를 순차적으로 계산하여 합함
> - Defective zone과 non-defective zone의 contrast를 최대화함
- Post-processing step에서는 local directional-based blurring 방법을 사용하여 fusion image의 background를 획득함
> - Sobel edge detector를 사용
> - Gradient direction image에서 image background를 획득
- Image background의 contrast를 증가시킨 뒤 thresholding을 통한 detection 수행
> - Detection stage를 3단계로 구성하고, 1단계부터 3단계까지 resolution을 줄여가며 detection 수행 (작은 결함 식별하고 큰 결함도 식별)

## 실험결과
- 스페인 벤츠 공장의 자동 품질 관리 시스템인 QEyeTunnel에서 실험 진행
- 성능이 좋았고 알고리즘의 수행 속도는 cycle time에 큰 영향을 주지 않았음

## 의견
- /