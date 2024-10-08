# Image Style Transfer Using Convolutional Neural Networks (CVPR, 2016)

[논문링크](https://openaccess.thecvf.com/content_cvpr_2016/html/Gatys_Image_Style_Transfer_CVPR_2016_paper.html)

<p align="center">
    <img width="700" alt='fig1' src="../img/gatys2016image.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 어떤 이미지의 style을 다른 style로 transfer하는 작업은 texture transfer problem의 일환으로 볼 수 있음
- Texture transfer의 목적은 target image의 semantic content는 유지하면서 source image의 texture를 입히는 것
- 이전의 texture transfer methods는 주로 non-parametric한 방법이었음
> - 이러한 방법들은 target image의 low-level features만을 사용하여 texture transfer를 수행한다는 한계가 있음
- Style transfer 알고리즘은 target image에서 objects와 같은 semantic image content를 추출할 수 있어야 하며, 추출된 semantic content를 source image의 style로 렌더링할 수 있어야 함
- 따라서, semantic image content와 그 style의 variation을 독립적으로 모델링하는 효과적인 image representations를 찾아야 함
- 충분한 labeled data로 훈련된 CNN은 여러 데이터셋 및 여러 visual tasks(i.e., texture recognition)에서 일반화될 수 있는 high-level image content를 학습함
- 본 논문에서는 CNN으로 학습된 generic한 feature representations을 natural images의 content와 style을 처리하고 변경하는 데 사용함
> - Image style transfer를 위한 새로운 알고리즘인 'A Neural Algorithm of Artistic Style'을 제안함

## 접근법
- Pre-trained VGG-Net을 backbone으로 사용함(VGG-19)
- Content representation을 학습하기 위해 MSE loss 기반의 reconstruction task 수행
> - Random white noise image를 모델에 입력하고, 각 layer에서 feature representations를 추출함
> - 임의의 original image를 모델에 입력하고, 각 layer에서 feature representations를 추출함
> - 두 representations의 MSE loss를 계산하여 white noise image에 대해 backpropagation 진행(content loss)
> - 이러한 과정을 반복하여 CNN의 각 레이어에서 original image와 동일한 features를 생성하는 random image를 update할 수 있음
> - 이때 네트워크의 후반부 레이어에서는 objects와 관련된 high-level content가 포착되지만 정확한 appearance를 반영하지는 못함
> - 반대로 네트워크의 전반부 레이어에서는 단순히 original image의 pixel values를 모방하려고 하는 경향이 있었음
> - 따라서, 후반부 레이어의 feature representations를 content representation으로 사용함
- Style representation을 학습하기 위해 texture information을 포착하기 위해 디자인된 feature space를 이용함
> - 각 layer에서 생성된 feature maps를 내적하여 feature correlations를 계산함
> - 여러 layer에서 계산된 feature correlations는 multi-scale의 texture information을 담고 있음(style representation)
> - Content loss와 마찬가지로 random noise image로부터 original image의 feature correlations를 reconstruction하도록 훈련시킴(style loss)
- Style transfer를 수행하기 위해 content representation과 style representation을 동시에 이용함
> - White noise image의 feature representation을 content representation과 style representation에 대해 동시에 최적화함

## 실험결과
- CNN을 사용하면 content와 style을 효과적으로 분리할 수 있음
> - 즉, content/style representation을 독립적을 조작하여 새롭고 흥미로운 image를 생성할 수 있음
> - 물론 완벽하게 분리되지는 않으며 style과 content는 trade-off 관계에 있음
- 촬영된 사진 content에 artwork의 style을 입히는 task를 통해 성능을 검증함

## 의견
- / 