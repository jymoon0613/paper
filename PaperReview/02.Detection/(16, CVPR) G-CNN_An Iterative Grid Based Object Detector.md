# G-CNN: An Iterative Grid Based Object Detector (CVPR, 2016)

[논문링크](https://www.cv-foundation.org/openaccess/content_cvpr_2016/html/Najibi_G-CNN_An_Iterative_CVPR_2016_paper.html)

<p align="center">
    <img width="500" alt='fig1' src="../img/najibi2016g.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 2-stage detector에서 region proposal 과정은 bottleneck으로 작용함
- 본 논문에서는 object proposals 없이도 SOTA에 근접한 detection performance를 기록할 수 있다는 것을 증명함
- 구체적으로, 규칙적으로 샘플링된 multi-scale grid를 반복적으로 업데이트하여 objects를 포함하는 boxes로 변환시킴 (G-CNN)

## 접근법
- 원본 이미지에 대해 고정된 크기의 multi-scale grid를 정의함
- 생성된 grid block을 하나의 bbox로 보고, ground-truth bbox와의 IoU가 0.2 이상인 bbox를 해당 ground-truth bbox로 할당함
- G-CNN은 할당된 bbox를 각자의 ground-truth bbox와 같아지도록 regression을 수행함
> - 이때 한 번에 regression을 수행하는 것이 아니라 여러 steps에 걸쳐서 업데이트함
> - 각 step에서, ground-truth bbox의 위치, 현재 bbox의 위치, 남은 step 수를 고려하여 target bbox를 정의하고, 정의된 target bbox에 대해 regression을 수행함
- G-CNN은 이러한 방식으로 모든 training steps, object classes의 regression loss를 종합하여 optimize됨

## 실험결과
- Faster R-CNN보다 5배 빠르지만 정확도는 비슷했음

## 의견
- / 