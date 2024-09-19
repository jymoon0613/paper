# Computer Vision Techniques in Manufacturing (IEEE T-SMC, 2022)

[논문링크](https://ieeexplore.ieee.org/abstract/document/9761203)

<p align="center">
    <img width="500" alt='fig1' src="../img/zhou2022computer.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 제조 영역에 computer vision(CV)가 활용될 수 있는 기회는 image-based metrology, manufacturing process interpretability, material structure analysis로 그룹화할 수 있음
- 적용되는 시나리오의 관점에서는 visual inspection(비전 검사), part identification(부품 식별), process control(공정 제어), robotic guidance mechanisms(로봇 가이드 메커니즘)으로 분류 가능함
- 본 논문에서는 CV가 복잡한 제조 영역에서 어떻게 활용되는지와 product lifecycle의 관점에서 적용가능한 CV applications에 관해 리뷰하고자 함

## 접근법
- 우선, Manufacturing-oriented CV system framework는 optical devices(lighting module, sensing module), hardware(manufacturing system, actuators), software(CV system, decision-making module), data(optical images, feature decriptions, decision signals)로 구성됨
> - Ligting module이 manufacturing system에 lighting을 제공하여 sensing module이 manufacturing system으로부터 images를 수집할 수 있도록 함
> - CV system은 sensing module로 수집한 digital images를 입력받아 탐지된 features 혹은 images에 대한 descriptions를 출력함 (detection, recognition, segmentation, 3D modeling 등 적용가능)
> - CV system의 처리 결과는 decision-making module로 입력되며, priority-based rules, heuristic algorithms, intelligent optimization algorithms 등을 통해 산출된 decision signals는 actuators의 actions를 control하는 데 활용됨
> - Actuator의 action sequence는 actuator와 manufacturing system간의 상호작용을 반영함
- CV는 제조 digital twin 개발에도 상당한 도움이 될 수 있음
> - 모델링과 시뮬레이션 기술을 활용하여 digital twin은 제조 시스템의 정보화 및 지능화에서 점점 더 중요한 역할을 수행하고 있음
> - 이미지 처리 및 CV 기술을 사용하여 제조 시스템의 이미지 정보를 실시간으로 포착하고, 이러한 이미지를 기반으로 시스템 상태에 대한 대략적인 정보를 추출함
> - 추출된 정보는 digital twin과 실제 시스템 간의 상태 일관성을 유지하는 데 도움이 됨
- Product lifecycle에 따라 manufacturing stages를 구분한다면 product design, modeling & simulation(M&S), planning & scheduling, production process, inspection & quality control, assembly, alignment, transportation로 나눌 수 있음
> - 그동안 게재된 papers를 보면 CV 기술은 주로 inspection, production, assembly에서 많이 사용됨
- Product desing과 M&S에 적용되는 대표적인 CV 기술은 기존 product의 2D images를 3D models로 reconstructing하는 것
> - 설계한 product에 대한 3D digital twins를 만들어 simulation, validation 해보는 것
> - 하지만 최근 CAD 소프트웨어의 발전으로 3D model reconstruction 과정이 별도로 없이 3D digital twins를 쉽게 생성하여 관리할 수 있음
- Product design에 대한 validation이 완료되면 production plan을 만들고 plan 실행을 위한 scheduling이 필요함
- Planning & scheduling에서 CV 기술은 용접 head의 경로 계획을 생성하는 등의 목적으로 사용됨
- Production process는 product의 전체 lifecycle에서 매우 중요한 stage임
> - 산업마다 production의 형태는 다르지만 일련의 operations로 raw materials를 product로 변환시킨다는 개념은 유사
- Production process에 사용되는 CV 기술은 part recognition, part classification, production process monitoring, robot guidance, 3D position measurement, production safety monitoring이 있음
> - Production Process Control: CV 기술은 여러 생산 공정을 제어하기 위해 적용 가능함 (e.g., 광물면 생산을 위한 용융 암석의 궤적 제어, 섬유의 높이 및 밀도 측정, 다결정 태양광 웨이퍼의 패턴 특징을 추출하여 부유 공정을 제어)
> - Robot Guidance: CV는 로봇 제어 및 human-robot 협동 과정에 적용 가능
> - Part Classification: 제품 및 부품 분류 과정에 CV 기술 적용
> - 3D Position Measurement: 나사 구멍 위치 등 객체의 3D 좌표/위치를 추정하는 데 사용
> - Production Safety Control: 작업자의 헬멧 착용 여부 식별, 제작 설정이 목표로하는 CAD 모델과 일치하는 지 여부를 파악하여 도구와 구성 요소 간의 충돌 회피 등 현재 상황 인식 및 발생 가능한 상황을 인식하여 대응 체제 구축에 CV 기술 사용 가능
- CV 기술은 quality control에 널리 적용되고 있고, visual inspection은 quality control의 매우 중요한 부분을 담당
- CV 기술은 적용되는 제조 산업에 따라 다르게 나타날 수 있음: mechanics(기계), automotive(자동차), textile(섬유), 3D printing(3D 프린팅) 등
> - Mechanics: 회전된 표면 검사, 결함 감지, 손상 부품 감지, 생산 공정의 원격 품질 검사 등에 CV 기술 사용
> - Automotive: 휠 얼라인먼트, 표면 품질 검사 등에 CV 기술 사용 가능
> - Vision 기반의 inspection systems는 보통 많은 양의 데이터를 수집하게 됨, 해결해야 할 중요한 과제는 수집한 데이터를 효과적으로 관리하고 활용하는 방법 (새로운 장치 혹은 알고리즘을 개발하는 것이 필수적)
- 제조된 각 parts를 assembly하는 과정에서도 CV 기술 사용 가능함
> - Automatic Assembly: 조립 자동화 과정에서 CV 기술 사용 가능 (w/ robot vision)
> - Assembly Quality Control: 조립 오류 검사 과정에서 CV 기술 사용 가능 (i.e., 부품 사이의 기하학적 위치에 대한 통계적 패턴 인식)
- 제조 공장의 transportation activities에서도 CV 기술 활용 가능
> - AGV: AGV의 경로 navigating 과정에서 CV 기술 사용 (i.e., path planning, object detection, obstacle detection)
> - Transportation systems에서 CV 기술이 마주할 수 있는 문제점은 images 간의 variation이 심하다는 것, 복잡한 illumination conditions로 인해 그림자 및 highlights가 발생, 더 robust한 CV 기반의 transportation systems를 구현하기 위해 denoising과 normalization 기술 개발이 필요
- 제조 현장에 적용되는 CV 기술의 대표적인 문제점은 구현이 어렵다는 것임
> - CV 기술의 발전 속도에 비해 제조 현장에 적용되는 CV 기술은 대부분 오래전의 것에 머물러 있음
> - 최신 연구 결과를 적용할 수 있을 만큼 충분한 resources를 보유한 large-scale 기업일수록 부서간 서로 다른 software, hardware systems를 사용하고 있는 경우가 많고 이러한 복잡함이 최신 CV 기술 적용을 어렵게 함
> - 또한, 최신 CV 연구는 일반적으로 실제 제조 시스템과는 달리 많은 양의 labeled data가 포함되어 있다고 가정하는 이상적인 문제를 해결하고자 하므로 최신 연구 결과를 실제로 활용하기에 괴리가 발생하기도 함
- Lighting condition의 제약 등에 의해 high-quality 데이터 수집이 어렵다는 문제도 있음
- 데이터 전처리가 어렵다는 문제도 있음
> - 제조 시스템에 더 많은 sensing devices가 도입되면서 더 많은 양의 정형/비정형 데이터가 수집되지만, 모든 데이터가 CV 시스템에 사용될만한 가치가 있는 것은 아님
> - 일부 기업에서는 수집된 데이터를 일정 기간 동안 데이터베이스에 임시 저장하고, 정기적으로 3개월 단위로 데이터를 삭제하는 방식을 선택함
> - 즉, 수집되는 데이터의 양이 많아지는 데 반해 데이터 전처리 메커니즘이 부족하여 데이터 저장 비용이 훨씬 비싸지고 데이터 처리 효율성이 낮아짐
- 데이터 labeling이 어렵다는 문제도 있음
> - 제조 현장에서 수집되는 데이터의 양이 많아지는 데 반해 수집된 raw data는 supervised learning을 위해 필요한 labels가 부족함
> - 대규모의 raw data에 일일이 label을 할당하는 작업은 매우 비용이 큼
> - 즉, non-labeled 데이터를 다룰 수 있는 효과적인 알고리즘 혹은 raw data에 자동으로 label을 할당하는 작업이 필요함
> - SimCLR와 같은 SSL이 해결방안이 될 수도 있음

## 실험결과
- /

## 의견
- /