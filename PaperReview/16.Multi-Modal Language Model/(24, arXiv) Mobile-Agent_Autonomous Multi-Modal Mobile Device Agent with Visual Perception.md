# Mobile-Agent: Autonomous Multi-Modal Mobile Device Agent with Visual Perception (arXiv, 2024)

[논문링크](https://arxiv.org/abs/2401.16158)

<p align="center">
    <img width="700" alt='fig1' src="../img/wang2024mobile.png?raw=true"></br>
    <em><font size=2>The framework of Mobile-Agent.</font></em>
</p>

## 연구목적
- 최근 Multimodal Large Language Models(MLLM)은 mobile device agent로서 각광받고 있음
> - Mobile device agent는 user instructions와 screen에 기반하여 명령을 수행함
> - 이를 위해 mobile device agent는 visual perception과 semantic understanding이 모두 가능해야 함
- 하지만 GPT-4V와 같은 최근의 MLLM은 visual perception 능력이 부족하여 효과적인 mobile device agent로 활용하기에는 부적절함
- 이를 해결하기 위해 GPT-4V에 user interface layout 정보가 포함된 XML 파일 혹은 HTML 코드를 추가로 입력하는 방법이 시도되었지만 모든 상황에서 이러한 추가 정보를 추출할 수 있는 것은 아니기 때문에 활용성이 떨어짐
- 따라서, 본 논문에서는 이러한 추가 정보 없이도 충분한 visual perception을 내포하고 있는 mobile device agent인 Mobile-Agent 프레임워크를 제안함
> - Mobile-Agent은 현재 화면의 스크린샷에만 의존하여 operation을 수행함
> - 이를 위해 detection 및 OCR 모델을 활용함
- 또한, Mobile-Agent의 성능을 평가하기 위해 최근의 주요 mobile Apps에서의 operations로 구성된 벤치마크인 Mobile-Eval을 제안함
> - Mobile-Eval은 다양한 난이도의 instructions로 구성됨

## 접근법
- Mobile-Agent 프레임워크는 SOTA MLLM인 GPT-4V, text localization을 위한 text detection module, icon localization을 위한 icon detection module로 구성됨
- 우선 agent가 screen 상의 특정 text에 접근하기 위해서는 해당 text가 screen의 어디에 위치하는지를 알아내는 과정, 즉, text localization이 필요하며 이를 위해 OCR tool을 사용함
> - Screen에 지정된 text가 중복으로 존재하는 상황, 지정된 text가 없는 상황 등 각 시나리오에 대한 agent의 동작을 지정함
- 또한, agent가 특정 icon을 클릭해야 하는 상황에서는 icon localization을 위해 icon detection tool과 CLIP을 사용함
> - Agent에게 클릭해야 하는 icon의 특징(e.g., 색, 모양)을 알려주는 동시에 Grounding DINO를 사용하여 화면 내의 모든 icon을 식별함
> - CLIP을 사용하여 식별된 icons와 descriptions 간의 similarity를 계산하여 클릭할 위치를 확정함
- Mobile-Agent는 App 열기, text/icon 클릭하기, 뒤로가기 등 8개의 동작을 수행할 수 있음
- 사용자가 instruction을 입력하면 Mobile-Agent는 순차적으로 operation을 수행함
> - 현재 화면에 대한 screenshot을 agent에게 전달함
> - Agent는 첫번째 동작부터 하나씩 완수하면서 instruction을 수행함 (self-planning)
> - 예를 들어, 'A라는 위치로의 경로를 찾아달라'는 명령에 대해 '지도App 열기', '검색창에 A위치 입력하기', '길찾기 icon 누르기' 등의 일련의 동작을 수행함
- Instruction을 수행하는 과정에서 예기치 못한 에러가 발생하는 경우에 대비하기 위해 self-reflection 기법을 설계함
> - Agent가 잘못된 동작을 수행하여 instruction을 더이상 따르지 못하는 경우 대체 동작을 수행하거나 현재 동작의 parameters를 변경하도록 함
> - Agent가 self-planning을 통해 모든 동작을 수행하고 난 뒤, agent는 일련의 동작 history, 현재 화면 screenshot, user instruction에 기반하여 결과를 평가하고, 만약 결과가 instruction과 다른 경우 agent는 다시 self-planning을 통해 동작을 수행함
- Mobile-Agent가 효과적으로 동작하도록 하기 위해 Observation, Thought, Action 세 가지로 구성된 prompt format을 지정함
> - Observation은 현재 screenshot과 과거 동작들에 대한 agent의 설명을 의미하며 이는 agent가 screenshot의 업데이트 현황을 파악하도록 하며 과거 동작들에 기반하여 오류를 식별하도록 함
> - Thought는 instruction과 Observation에 기반하여 다음 동작에 대한 agent의 판단을 의미함
> - Action은 Thought에 기반하여 agent가 8개의 동작 중 하나와 그 parameter를 결정하는 것을 의미함

## 실험결과
- Android OS에서 평가를 진행함
- Mobile-Agent를 평가하기 위해 Mobile-Eval 벤치마크를 제안함
> - 유튜브, 틱톡을 포함한 10가지의 mobile Apps에서 평가가 이루어짐
> - 각 App에 대해 서로 다른 난이도의 3가지 instructions를 할당함
> - 평가 metric으로는 Mobile-Agent가 instruction을 완수했는지를 의미하는 Success, instructions를 수행하는 각 step의 정확도를 의미하는 Process Score, 같은 instruction에 대해 인간이 수행한 동작(optimal)의 수와 agent가 수행한 동작의 수를 비교하는 Relative Efficiency, instruction을 구성하는 동작 중 agent가 완료한 동작의 수를 의미하는 Completion Rate로 구성
- Mobile-Eval에서, Mobile-Agent는 모든 metrics 상 준수한 성능을 보여줌
- Case-study를 통해, Mobile-Agent의 self-planning 및 self-reflection이 잘 기능함을 알 수 있었고, 복잡한 instructions에 대해서도 준수한 성능을 보여주었음

## 의견
- /