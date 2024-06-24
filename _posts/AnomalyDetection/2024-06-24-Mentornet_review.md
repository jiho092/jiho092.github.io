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

# 2.  Preliminary on Curriculum Learning
Deep learning 학습은 단순한 패턴을 먼저 학습하는 패턴이 존재한다. 따라서 Noise label을 가진 데이터에 대해서는 비교적 큰 Loss를 가지고 있다. MentorNet은 이 특징을 이용한다.

## 2.1 Notation

* $\mathcal{D} = {(x_1,y_1),...,(x_n,y_n)}$ : data set
* $y_i \in {0,1}^m$ : m개의 class에 대한 noise label vector
* $g_s(x_i,w)$ : StudentNet의 discriminative function
* $v \in \mathbb{R}^{n x m}$