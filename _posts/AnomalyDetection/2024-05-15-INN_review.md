---
layout: single
title: "INN method paper review" #제목
excerpt : ""
categories: 
    - anomalydetection #카테고리설정
tag: 
    - ["image","anomaly", "ood","noisy label"] #테그

date: 2024-05-15
last_modified_at: 2024-05-15
#classes: wide    
---

[INN: A Method Identifying Clean-annotated Samples via Consistency Effect in Deep Neural Networks](https://arxiv.org/pdf/2106.15185){: .btn .btn--primary}

# 0. Abstract

noise label 문제를 다룰 때, 좋은 성능을 보였던 모델들은 memorization effect를 loss로 사용하는 전략을 사용했다.
그러나 memorization effect 방법은 train epoch에 민감하고, 데이터가 심하게 오염되거나 imbalance한 경우에는 좋은 성능을 보이지 못했다.

그래서 이 논문에서는 noise가 섞인 데이터로 부터 clean label을 재정의하는 새로운 기법인 INN(Integration with the Nearest Neighborhoods) 기법을 제시한다.
이 방법은 clean label의 neighbor region의 패턴이 training epoch에 상관없이 noisy label data와 일관되게 다르다는 것으로 부터 시작한다.

INN method는 계산량이 많지만 안정적이고 좋은 성능을 보인다.

# 1. Introduction

clean labeled data를 얻기 위해 전문가가 annotaition을 하는 것은 비용적으로, 시간적으로 많은 비용이 발생한다.

반면 인터넷 search를 통해 data를 수집하면 적은 비용으로 data를 수집할 수 있지만, 정확도가 떨어진다.

그래서 비용도 줄이고 정확도도 올리기 위해서 Noise label 문제를 해결할 필요가 있다.


## 1.1 Memorization effect Method
Noise label 문제를 해결하는 방법에는 여러가지가 존재하는데, 그 중 대표적인 방법이 Memorization effect를 사용하는 방법이다.

Memorization effect는 학습 과정에서 clean label sample에 대해서는 Loss가 빠르게 감소하고, noisy label sample에서는 Loss가 느리게 감소하는 것을 이용한다.
이 방법은 간단하고, 성능이 좋기 때문에 그동안에 많은 연구에서 사용해왔다.

그러나 이 방법에는 몇가지 약점이 존재한다.
1. training epoch(iteration)를 어느 정도로 설정해야하는지 알기 어렵다.
2. data의 오염 정도가 심하거나 class imbalance가 존재할 때 좋은 성능을 보이지 못한다.


## 1.2 INN Method
논문에서는 이러한 문제를 해결하기 위해 INN method를 제안한다.

이 방법은 Clean label sample과 Noise label sample의 neighbor regions이 training epoch과 관계없이 일관되게 다르다는 관측으로 부터 시작한다.

이 논문에서는 이것을 consistency effect라고 부른다. 

INN method를 그림으로 보면 아래 그림과 같다.

![INN](/assets/images/anomalydetection/INN/INN_figure.png)

그림을 보면 Noisy labeled sample의 경우 Neighbor region의 평균 결과값(consistency effect)이 작은 것을 확인할 수 있다.

### 1.2.1 Properties of INN

1. small-loss method보다 계산량이 많다.
2. 심한 데이터 오염이나 data imbalance 문제에서 좋은 성능을 보인다.
3. 다양한 supervised learning task에서 안정적이고 우월한 성능을 보인다.

# 2. Related works

실험에서 결합하여 사용한 기법들을 소개해준다.

1. Deep k-NN
2. MentorMix
3. MixUp
4. MentorNet

# 3. Integration with the nearest neighbors

## 3.1 Notation & Definition

* $x \in \mathbb{R}^d$ : input vector
* $y \in [K]$ : observe label
* $y^* \in [K]$ : ground-truth label

if $y = y^*$, then sample $(x,y)$ is cleanly label

if $y \neq y^*$, then sample $(x,y)$ is noisy label

* $\mathcal{D}^tr = {(x_i,y_i)}$ : training dataset
* $\mathcal{C}^tr = {(x,y) \in \mathcal{D}^tr : y = y^*}$ : Clean sample
* $\mathcal{N}^tr = {(x,y) \in \mathcal{D}^tr : y \neq y^*}$ : Noisy sample
* $\tilde{x}$ : nearest neighbor training input of $x$

## 3.2 Consistency effect

* $x^m = (x + \tilde{x})/2$
* $\tilde{x}$ : nearest neighbor training input of $x$
* $h(\tilde{x} ; \hat{\eta})$ is most close to $h(x ; \hat{\eta})$
* $h(..,\hat{\eta})$ : model layer중 끝에서 두번째 output

이 때 $x^m$ 은 $x$ 의 neighbor region에 위치한다. 이 논문에서는 clean label에 의해 얼마나 prediction value가 다른지 조사하였고, prediction model을 추정하였다.
또한 아래 4개의 기댓값을 정의하였다.

![INN](/assets/images/anomalydetection/INN/image2.png)

$E_{cor}, E_{inc}$ 은 각각 clean, noisy data의 예측값을 의미하고, $E_{cor}^m, E_{inc}^m$ 은 negihbor region의 prediction value의 기댓값을 의미한다.

![INN](/assets/images/anomalydetection/INN/image3.png)

그림 2를 보았을 때, memorization effect($E_{cor}, E_{inc}$ 가 이른 epochs에서 많이 달라짐)을 확인할 수 있다. 그러나 epoch이 늘어날 수록 그 차이는 감소하는 것을 확인할 수 있다.
이 의미는 training이후 각 sample에서 $f_y(x)$ 값을 비교하여 clean, nosiy를 구별하기 어렵다는 것이다.

epoch을 잘 결정한다면 좋은 성능을 보일 수 있지만, 현실적으로 epoch을 결정하는 것이 어렵다.

반면 두 neighbor region의 prediction의 기댓값을 확인해보면 training epoch과 관계없이 충분히 차이가 나는 것을 확인할 수 있다. 즉, clean data와 noisy data를 구분할 수 있다. 논문에서는 이 현상을 **consistency effect** 라고 부른다. (**주황색 실선과 점선**)

이 현상은 두가지 이유로 발생한다.

(1). $(x,y)$ 가 noisy sample일 떄, label과 $\tilde{x}$ 의 label이 일치하지 않는 경향이 있고, 이것이 작은 예측값 $f_y(x^m)$ 을 산출한다.
(2). 만약 $y, \tilde{y}$가 같을지라도, $\tilde{x}$ 가 $x$에 가깝지 않을 수 있다.

따라서 $\tilde{x}$ 와 $x$ 사이에 $f_y(.)$ 가 작아지는 region이 존재한다.

## 3.3 INN method

이 절에서는 consistency effect를 이용한 INN method에 대해 설명한다.

training epochs $T^{inn}$ 에 대해 data  $\mathcal{D}^tr$을 loss function $l$로 학습시킨 prediction model을 $f(..,\hat{\theta})$ 라하자.

이 논문에서는 loss function으로 **Mixup** objective function을 사용하였다.
이 때 clean data인지 noisy data인지 식별하기 위한 함수(점수)는 다음과 같다.

$I(x;\hat{\theta},\tilde{x})$ = $f_y(x^m;\hat{\theta})$

추가적인 실험을 통해 이 함수(점수)를 수정해 나갔다.

(1). Consitency effect를 충분히 활용하기 위해, $x, \tilde{x}$ 사이 전체 구간에서 적분한다.

- consistency effect는 $x^m$ 보다 $x, \tilde{x}$ 사이의 input vector에서 많이 발생하기 때문에

![INN](/assets/images/anomalydetection/INN/image4.png)

(2). multiple neighbor sample을 사용하는 것이 clean label을 더 정확히 식별하는데 도움을 준다.

![INN](/assets/images/anomalydetection/INN/image5.png)

(3). Trapezoidal rule(사다리꼴 룰)

![INN](/assets/images/anomalydetection/INN/image6.png)

- $\tilde{\chi} = (\tilde{x_1},...,\tilde{x_L})$ 이 L nearest neighbor set이라고 할 때,
위 (1),(2)에 따라 적분을 했을 때 정확도가 증가하고, L의 수가 커질 수록 정확도가 증가하는 것을 확인할 수 있다. 따라서 최종적으로 수정한 Score function은 다음과 같다.

![INN](/assets/images/anomalydetection/INN/image7.png)