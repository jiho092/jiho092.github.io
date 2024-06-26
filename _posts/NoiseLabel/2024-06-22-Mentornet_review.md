---
layout: single
title: "MentorNet paper review" #제목
excerpt : ""
categories: 
    - NoiseLabel #카테고리설정
tag: 
    - ["classification","anomaly", "ood","noisy label"] #테그

date: 2024-06-22
last_modified_at: 2024-06-22
#classes: wide    
---

[MentorNet: Learning Data-Driven Curriculum for Very Deep Neural Networks on Corrupted Labels](https://arxiv.org/pdf/1712.05055){: .btn .btn--primary}

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

![Image1](/assets/images/anomalydetection/Mentornet/image1.png){: .align-center}

* $G$ : curriculum, parameterized by $\lambda$

![Image1](/assets/images/anomalydetection/Mentornet/image2.png){: .align-center}

논문에서는 $v$ 를 위와 같이 loss가 $\lambda$ 보다 작으면 1로 두어 select하고, $\lambda$ 보다 크다면 0으로 두어 select하지 않는다. threshold가 작을 때는 small loss sample이 고려되고, 클 때는 larger loss sample이 고려된다.

# 3. Architecture and Algorithm
![Image1](/assets/images/anomalydetection/Mentornet/image3.png){: .align-center}

논문에서는 Curriculum learning **(CL)** 방법으로 self-paced learning방법을 사용한다. 이 방법은 $\lambda$ 이하의 loss를 가지는 데이터만 학습을 진행하는 방법이다. 논문에서는 가중치를 0과 1로 두어, clean data에 대해서는 학습을 진행하고, noise data에 대해서는 학습을 진행하지 않는다. 따라서 CL 방법을 MentorNet에서는 Noise인지 Clean데이터인지 판별을 하고, 이후 학습을 진행하는 것을 볼 수 있다.
알고리즘은 다음과 같다.

![Image1](/assets/images/anomalydetection/Mentornet/image4.png){: .align-center}


# 4. Experiment
Noise 종류에는 크게 두가지가 존재한다. 하나는 class의 labeling이 섞이는 경우이고, 다른 하나는 label이 random(uniform)하게 확률적으로 섞이는 경우이다. 이 논문에서는 실험을 위해 임의적으로 Noise를 부여한 경우(CIFAR-10,100, ImageNet)와 real-world에서의 noise label을 가지는 경우(Webvision)를 다루고 있다.

## 4.1 Experiment on controlled corrupted labels
이 장에서는 임의적으로 Noise label을 생성하여 MentorNet을 검증한다. 이 때, unifrom random 확률 $p$ 에대해 label을 변경하는데 0.2, 0.4, 0.8인 경우에 대해 학습을 진행하고 validation dataset은 clean한 상태를 유지한다.

* Dataset
    
    1. CIFAR-10 : (32X32, color, classes : 10), 50,000 training dataset, 10,000 validation dataset
    2. CIFAR-100 : (32X32, color, classes : 100), 50,000 training dataset, 10,000 validation dataset
    3. ImageNet : (299X299, color, classes : 1000), 1.2 milion training set, 50k validation set

* StudentNet(CNN 사용)

    1. resnet-101
    2. interception_resnet v2

아래 표는 Clean training data로 학습을 진행했을 때의 정확도와 parameter 수이다.
    
![Image1](/assets/images/anomalydetection/Mentornet/image5.png){: .align-center}

baseline으로는 $l2$ weight decay, drop out(0.2~0.9)로 학습된 StudentNet을 사용하고 Clean dataset을 이용하여 최적의 hyperparameter를 찾는다.

* Model

    논문에서 제안한 모델은 두 가지가 있다. 하나는 Predifined curriculum을 사용하여 학습한 MentorNetPD이고, 다른 하나는 data-driven curriculum을 사용한 MentorNetDD이다. 이후 결과를 확인하면, MentorNetDD가 좋은 성능을 보이는 것을 확인할 수 있는데, 이 방법은 CIFAR-10에서 random하게 5,000개의 true image를 sampling 하여 training시킨다.
    
이에 대한 결과는 아래 표와 같다.

![Image1](/assets/images/anomalydetection/Mentornet/image6.png){: .align-center}



Resnet-101을 사용했을 때와 Inception StudentNet을 사용하였을 때, 기존 방법들에서는 대부분 Self-Paced 방법이 좋은 성능을 보여왔고, MentorNet을 통해 개선이 되는 결과를 확인할 수 있다. 특히 data-driven curriculum을 사용한 MentorNetDD에서 대부분 가장 좋은 성능을 보인 것을 확인할 수 있다. 기존의 그러나 noise probability를 0.8로 설정하였을 때는, 아직 성능 개선이 필요해보인다.

![Image1](/assets/images/anomalydetection/Mentornet/image7.jpg){: .align-center}

그림 (a)는 training epoch당 validation Error를 표현한 그림이고, (b)는 mini-batch당 Training Error이다. 그림을 보면 모두 학습이 진행될 수록 Training Error가 줄어든다. Validation Error를 확인해보면 MentorNetDD의 경우에는 epoch이 증가하더라도 Error가 증가하지 않는 것을 볼 수 있다. 그러나 다른 method를 보면 특정 epoch 이상에서는 Error가 증가하는 것을 확인할 수 있다. 따라서 MentorNet이 noise data에 대해 robust하다는 결과를 알 수 있다.

![Image1](/assets/images/anomalydetection/Mentornet/image8.jpg){: .align-center}

이 표는 40%의 Noise를 가질 때 다른 Noise problem을 다룬 방법들과 비교한 표인데, MentorNet이 SOTA보다 좋은 성능을 보인 것을 확인할 수 있다.

![Image1](/assets/images/anomalydetection/Mentornet/image9.jpg){: .align-center}

ImageNet에 대해서도 기존 방법들보다 성능이 향상된 것을 확인할 수 있다.

논문에서는 real-world dataset인 WebVision에 대해서도 성능이 향상된 것을 확인할 수 있다. 해당 내용은 생략한다.

# 5. Conclusion

이 논문에서는 Data에 Noise가 존재할 때, Robust하게 훈련을 진행할 수 있는 방법인 MentorNet을 제시하였으며, epoch과 다른 정보들과 관계없이 overfitting을 제어할 수 있는 효과를 보이고, 실제 Noise data로 학습했을 때, 다른 모델들에 비해 좋은 성능을 보이는 결과를 도출하였다.
