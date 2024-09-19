# Improving CLIP Training with Language Rewrites (NIPS, 2024)

[논문링크](https://proceedings.neurips.cc/paper_files/paper/2023/hash/6fa4d985e7c434002fb6289ab9b2d654-Abstract-Conference.html)

<p align="center">
    <img width="800" alt='fig1' src="../img/fan2024improving.png?raw=true"></br>
    <em><font size=2>Illustration of our proposed in-context learning based strategy for language rewriting.</font></em>
</p>

## 연구목적
- CLIP의 image encoder는 augmentations가 적용된 iamges를 처리하지만 text에는 훈련 과정 중 아무런 변화를 주지 않음
  - Image encoder는 하나의 이미지로부터 augmented된 images를 입력으로 받지만, 이때 text encoder는 항상 같은 text를 처리함
- 이러한 input asymmetry는 두 가지 문제를 유발함
  - (1) Augmented된 images는 항상 같은 text signals를 받으므로, image encoder는 language side로부터 제한된 supervision을 제공받음
  - (2) Augmented된 images에 대해 text encoder는 항상 같은 text를 처리하므로 text overfitting의 위험이 있음
- 본 논문에서는 LLM을 사용하여 text rewriting을 하는 방식으로 훈련 과정 중 text에 변화를 주고자 함
- 이러한 아이디어에 기반하여 기존 CLIP의 성능을 개선한 language augmented CLIP(LaCLIP)을 제안함

## 접근법
- LLM에게 text rewriting의 올바른 형식을 알려주기 위한 몇개의 예시 템플릿을 생성함 (meta-input-output pairs)
  - Chatbots, human rewriting 등 여러 방법을 사용하여 예시를 생성
- LLaMA-7B를 text rewriting을 위한 LLM을 사용함
- Llama에 주어진 image descriptions를 rewriting하라는 명령과 함께 미리 생성한 meta-input-output pairs 중 3개를 랜덤하게 선정하여 예시로 제공함
- 이러한 방식으로 Llama는 하나의 이미지 당 4개의 augmented captions를 생성하며, 생성된 augmented captions를 CLIP 훈련에 사용함
    - 훈련 과정에서 주어진 이미지에 대한 4개의 서로 다른 captions 중 하나를 랜덤하게 선택함

## 실험결과
- Zero-/Few-shot setup에서 기존 CLIP 대비 대부분 향상된 성능 기록
- Linear probing에서도 기존 CLIP 대비 대부분 향상된 성능 기록

## 의견
- /
*/