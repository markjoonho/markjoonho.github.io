---
title: "[arXiv 2017] Focal Loss for Dense Object Detection"
description: "Reshaping the standard cross entropy loss."
pubDate: "Sep 10 2022"
heroImage: "/img/post1_fig11.png"
badge: "Paper Review"
---

## 0. Abstract
Object Detection은 이미지 내의 foreground 영역을 찾아내고, IoU(Intersection over Union) threshold에 따라 positive,negative sample로 나눔.


현재 accuracy가 높은 object detector의 대부분은 R-CNN기반 two-stage detector 사용.

- Two-stage detector의 경우 one-stage에 비해 속도 느림, 성능 높음.

- One-stage detector의 경우 two-stage detector에 비해 속도 빠름, 성능 낮음.


One-stage detector의 성능 저하 원인을 class imbalance라고 함. 


- 물체 vs 배경을 비교 했을때 배경이 일반적으로 더 많음.


위 문제 해결하기 위해 제안한 방식이 cross entropy loss를 reshaping 한 Focal Loss.

- Focal Loss의 효율성을 평가 하기 위해 간단한 dense detector인 RetinaNet 소개.

- RetinaNet과 Focal Loss 함께 사용 시 이전 one-stage detector와 비슷한 스피드와 함께 sota two-stage detector와 비슷한 성능 보여줌.



## 1. Introduction

현재 많은 sota object detector 모델들은 two-stage 형태임.

- first stage에서 sparse set of candidate object locations 생성 (region proposal).

- second stage에서 region proposal을 바탕으로 CNN을 이용한 object detection (foreground vs background)


Two-stage 모델이서는 왜 높은 accuracy가 나올까?

- first stage에서 object가 존재할 확률이 높은 region proposal를 생성, one-stage보다 class 불균형 문제가 덜 민감.

- second stage에서 fixed forground-to-background ratio (1:3) 혹은 Online Hard Example Mining (OHEM)을 사용하여 object와 background의 비율 유지. (sampling heuristic) 

One-stage 모델들은 어떻게 작동 할까?

- One-stage detector는 CNN에서 나온 Feature map에서 grid 형태로 나누어서 localization 진행.

- 이를 위해 미리 정의한 box(anchor box)를 사용하여, anchor box에 scales, aspect ratio를 적용한 dense sampling 사용.


One-stage 모델에서는 비슷한 accuracy 내기 힘들까?

- 정확도가 낮은 이유 중 하나로, 기존에 있던 class 불균형 문제(Object vs Background)가 심화 되는데 있음.

- 이미지 전체를 대상으로 dense sampling 진행으로 인한 class imbalance 문제가 심해짐.


본 논문에서 One-stage model의 해결책 제시

- Focal Loss & RetinaNet


Focal Loss의 $$\gamma$$를 사용하게 되면, ground truth에 대한 probability가 낮을 수록 loss를 높게 주고 probability가 높을 수록 loss를 낮게 주는 걸 알 수 있다. (Figure 1 참고)

- gamma가 0보다 커질수록 잘 검출한 물체와 아닌 물체 사이의 loss 값의 차이를 더욱 분명하게 만듦.

- Focal Loss는 학습 시에 background보다 object 검출에 집중할 수 있게 도움.

- Focal Loss가 기존의 방법(sampling heuristics, OHEM)보다 더 효과적이고 간단한 것을 보여줌.


![Alt text](/img/post1_fig1.png)

Focal Loss의 성과를 효과적으로 보여주기 위해, RetinaNet 제작 (figure2 참고).


- RetinaNet는 anchor box를 사용하는 FPN & Focal Loss 적용. (뒤에 다시 다룸)

![Alt text](/img/post1_fig2.png)

## 3. Focal Loss

학습시에 생기는 foreground와 background 간의 극심한 class imbalance를 해결하기 위해 개발됨. (e.g. 1:1000 과 같이 object와 background의 비율)

- 기존 binary cross entropy(CE) loss는 아래와 같은 식.

![Alt text](/img/post1_fig3.png)

- notation의 편리를 위해 P_t를 아래와 같이 적용.
- 여기서 P_t는 해당 class가 존재할 확률.

![Alt text](/img/post1_fig4.png)

- 따라서, 아래와 같은 식 성립.

![Alt text](/img/post1_fig5.png)

- Cross entropy의 특징으로 인해 P_t가 0.5보다 커도 loss가 생김.
- 이 의미는 box에 물체가 존재할 확률이 50% 넘어가도 loss가 꽤 생긴다는 점.
- 이로 인해 쉬운 object (P_t가 0.5보다 큼)가 많으면, 로스가 계속해서 쌓이기 때문에 학습이 잘 안될수가 있음.

#### 3.1. Balanced Cross Entropy

$\alpha$-balanced Cross Entropy(CE) Loss: weighted factor인 $\alpha$ 를 CE에 추가.

![Alt text](/img/post1_fig6.png)

- Object class:  $\alpha$ 를 0 ~ 1 사이로 선정
- Background class: 1 - $\alpha$
- $\alpha$-balanced Cross Entropy(CE) Loss의 장점: positive와 negative의 차별성을 표현 할 수 있음.
- $\alpha$-balanced Cross Entropy(CE) Loss의 단점: P_t > 0.5 (easy) 와 P_t < 0.5 (hard)의 차별성 표현 어려움.

#### 3.2. Focal Loss Definition

$\alpha$-balanced Cross Entropy(CE) Loss의 단점을 해결 하여 easy example과 hard example중 hard example에 초점을 주려 $\alpha$-balanced Cross Entropy(CE) Loss를 수정.

![Alt text](/img/post1_fig7_1.png)

- CE loss에 modulating fact를 추가. 
- P_t 값이 작을때, modulating factor가 1에 가까워 지면서 loss 커짐. (hard example)
- P_t 값이 클떄, modulating factor가 0에 가까워 지면서 loss 작아짐. (easy example)
- $\gamma$ 커질수록 modulating factor 영향 커짐.

> modulating factor로 easy hard 구분 세기 결정 가능. (논문에서는 2가 최고 라고 함.)

![Alt text](/img/post1_fig7_2.png)

- $\alpha$ 값이 추가된 focal loss.
- easy와 hard 뿐만 아니라 positive와 negative 차별성도 표현 가능.
- $\alpha$ 사용시 성능 더 좋음.

## 4. RetinaNet Detector

![Alt text](/img/post1_fig8.png)

- RetinaNet은 Feature Pyramid Network(FPN) backbone과 두개의 subnet(class & box regression)을 사용함.
- (a) feedforward로 ResNet 사용.
- (b) ResNet에서 FPN backbone 사용하여, multi-scale convolutional feature pyramid 생성. -> anchor box 생성.
- (c) anchor box의 class 예측 하는 class subnet.
- (d) anchor box와 GT box를 비교하여 regression 진행하는 box subnet.

## 5. Experiment

#### Focal Loss

- $\gamma$ 에 따른 최적의 $\alpha$ 값을 찾음.
- $\gamma$ 가 실제로 더 중요했음.

![Alt text](/img/post1_fig9_1.png)

### Focal Loss Analysis
- Focal Loss에 대한 이해를 위해 수렴한 모델들의 loss분포를 분석.
- ResNet-101 백본과 600픽셀 이미지, $\gamma$ = 2.0으로 학습.
- 학습 결과에 대한 Focal loss를 Foreground와 Background로 나누어서 누적분포함수를 그림.
- Foreground에 대한 누적분포 함수를 보면 $\gamma$ 의값에 크게 영향을 받지 않음.
- FL을 통해 효과적으로 Easy negatives의 영향을 무시할 수 있고 오직 Hard negative example에 집중할 수 있음.
![Alt text](/img/post1_fig9_2.png)

#### Focal Loss vs OHEM

- OHEM도 잘못 분류된 example을 강조하지만 Focal Loss와 다른점은 easy example을 고려하지 않음.
- RetinaNet-101과 Focal loss 사용하였을때 36.0 AP 보여줌.
- RetinaNet-101과 OHEM 사용했을때 32.8 AP 보여줌.

> OHEM이 FL보다 좋은 성능 내지 못함.

![Alt text](/img/post1_fig10.png)

#### 기타 실험 결과들
- 

![Alt text](/img/post1_fig11.png)

![Alt text](/img/post1_fig12.png)


## 6. Conclusion

- One-stage detector는 class imbalance 문제가 있었고 이를 해결하기 위해 focal loss를 사용하여 효율적으로 해결함.