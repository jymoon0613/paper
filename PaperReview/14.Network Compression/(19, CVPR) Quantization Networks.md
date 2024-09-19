# Quantization Networks (CVPR, 2019)

[논문링크](https://openaccess.thecvf.com/content_CVPR_2019/html/Yang_Quantization_Networks_CVPR_2019_paper.html)

<p align="center">
    <img width="600" alt='fig1' src="../img/yang2019quantization.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- Full-precision networks를 low-bit networks로 quantizing하는 방식은 approximation-based와 optimization-based의 두 가지로 구분 가능함
> - Approximation-based approaches: forward pass 과정에서 full-precision values를 low-bit values로 추정함. 하지만, 이 경우 backward pass가 가능하도록 gradient를 임의로 추정하는 과정이 필요하고 (binarization 과정은 미분불가능), 이는 gradient mismatch problem을 유발함
> - Optimization-based approaches: gradients에 대한 추정을 하지 않기 위해, 네트워크의 quantization 과정을 discretely constrained optimization 문제로 치환함. 하지만 이는 weights를 quantization하는 데만 적합하고 매 iteration마다 optimization을 푸는 과정은 computational complexity가 큼
- 만약 quantization을 activation functions와 유사한 non-linear한 function으로 치환할 수 있으면 (backward pass가 바로 가능하여) 별도의 gradients 추정 과정이 필요 없을 것임
- 본 논문에서는 quantization을 미분가능한 non-linear function으로 치환함
> - Learnable biases와 scales를 사용하는 Sigmoid functions의 집합으로 대체

## 접근법
- Quantization operation은 continuous한 inputs을 discrete한 interger numbers로 매핑함
- Quantization 과정은 여러 binary quantizations의 조합으로 표현될 수 있음
> - Combinations several unit step functions with specified biases and scales
- 본 논문에서는 continuous relaxation method를 사용하여 unit step functions를 sigmoide function으로 대체 (soft quantization function)
> - 미분가능하여 gradient mismatch 문제 발생하지 않음
- Soft quantization function으로 학습이 끝난 후, inference에서는 unit step function을 다시 사용

## 실험결과
- AlexNet, ResNet에 제안하는 방법으로 quantization 수행
- 제안하는 quantiztion network는 weight quantization 및 activation quantization 모두에서 기존의 SOTA를 능가
- SSD에 제안하는 방법으로 quantization 수행했을 때 detection task에서도 다른 SOTA quantization methods 능가

## 의견
- /