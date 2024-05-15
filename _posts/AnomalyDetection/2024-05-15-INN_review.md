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

