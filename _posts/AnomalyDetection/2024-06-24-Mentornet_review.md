---
layout: single
title: "MentorNet paper review" #제목
excerpt : ""
categories: 
    - anomalydetection #카테고리설정
tag: 
    - ["classification","anomaly", "ood","noisy label"] #테그

date: 2024-06-24
last_modified_at: 2024-06-24
#classes: wide    
---

# 1. Introduction
supervised learning에서 학습을 진행할 때에는 데이터가 정확한 Label을 가진다고 가정한다. 그러나 현실에서 얻어지는 데이터에는 Noise가 포함될 수 있다. 이러한 경우, 모델의 수렴속도가 오래 걸리지만 결국은 모델이 수렴하게 된다. 즉, 잘못된 데이터도 모두 학습을 시키는 것이기 때문에, overfitting이 되어 모델의 성능이 떨어질 수 밖에 없다. 이 논문에서는 corrutpted label 문제를 CNN에서 어떻게 극복하고 성능을 향상시킬수 있는지 연구하고, MentorNet을 제안한다. 

딥러닝은 과적합의 문제를 가지고 있다. 모든 train data를 학습하기 때문에 잘못 labeling된 데이터가 있다면 성능이 떨어질 수 밖에 없다. 

# 2.  Preliminary on Curriculum Learning
Deep learning 학습은 단순한 패턴을 먼저 학습하는 패턴이 존재한다. 따라서 Noise label을 가진 데이터에 대해서는 비교적 큰 Loss를 가지고 있다. MentorNet은 이 특징을 이용한다.
이 논문은 one-hot labeling에 대해 다룬다.

## 2.1 Notation

* $\mathcal{D} = [{(x_1,y_1),...,(x_n,y_n)}]$ : data set
* $y_i \in {0,1}^m$ : m개의 class에 대한 noise label vector
* $g_s(x_i,w)$ : StudentNet의 discriminative function
* $v \in \mathbb{R}^{n \times m}$

![Image1](/assets/images/anomalydetection/Mentornet/image1.png)

* $G$ : curriculum, parameterized by $\lambda$

![Image1](/assets/images/anomalydetection/Mentornet/image2.png)

논문에서는 $v$ 를 위와 같이 loss가 $\lambda$ 보다 작으면 1로 두어 select하고, $\lambda$ 보다 크다면 0으로 두어 select하지 않는다. threshold가 작을 때는 small loss sample이 고려되고, 클 때는 larger loss sample이 고려된다.

# 3. Architecture and Algorithm
![Image1](/assets/images/anomalydetection/Mentornet/image3.png)

논문에서는 Curriculum learning **(CL)** 방법으로 self-paced learning방법을 사용한다. 이 방법은 $\lambda$ 이하의 loss를 가지는 데이터만 학습을 진행하는 방법이다. 논문에서는 가중치를 0과 1로 두어, clean data에 대해서는 학습을 진행하고, noise data에 대해서는 학습을 진행하지 않는다. 따라서 CL 방법을 MentorNet에서는 Noise인지 Clean데이터인지 판별을 하고, 이후 학습을 진행하는 것을 볼 수 있다.
알고리즘은 다음과 같다.

![Image1](/assets/images/anomalydetection/Mentornet/image4.png)


# 4. Experiment
