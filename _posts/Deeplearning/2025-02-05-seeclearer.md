---
layout: single
title: "See Clearer at Night: Towards Robust Nighttime Semantic
Segmentation through Day-Night Image Conversion" #제목
excerpt : ""
categories: 
    - deeplearningCV #카테고리설정
tag: 
    - [] #테그

date: 2025-02-05
last_modified_at: 2025-02-05
classes: #wide    
---
[See Clearer at Night Towards Robust Nighttime Semantic Segmentation through Day-Night Image Conversion](https://arxiv.org/pdf/1908.05868){: .btn .btn--primary}


# Abstract

&nbsp;&nbsp; 이 논문에서는 Night image에 대해 Semantic Segmentation 성능을 향상 시키는 framework에 대해 제안하고 있다. Semantic Segmentation의 경우 주간 이미지에서는 충분히 성능이 잘 나오고 있지만, annotated dataset이 부족한 야간 이미지와 같은 경우에는 성능이 크게 저하되고 있다. 논문의 저자들은 GAN model을 base로 하여 주간 이미지로 사전학습된 모델에 야간 이미지를 주간 이미지로 변환하여 성능을 확인한다. 정량평가 지표로는 IoU, Pixel Acc를 사용하고, 생성된 야간 이미지의 비율에 따른 성능 또한 비교한다. 제안된 framework는 주행 분야에서 visual perception 최적화를 할 뿐만 아니라, 다양한 navigational assistance system에도 적용이 가능하다.

# 1. Introduction

&nbsp;&nbsp; object detection이나 semantic segmenetation과 같은 Vision task는 자율주행이나 보안 모니터링에서 키 포인트이다. CNN은 최근 컴퓨터적 자원과 거대한 데이터 셋 덕분에 이러한 방법들을 발전시켜왔다. semantic segmentation에서는 **RADAR와 LIDAR** sensor를 사용하는데, 이 중 두 번째 선택지로 만들어 복잡한 multi sensor fusion을 통해 장면 인식을 자유롭게 한다. CNN에서의 PSPNet, RefineNet, DeepLab, ACNet과 같은 SOTA 모델은 매우 높은 정확도를 가지고 있다. 논문에서는 자율주행, 보안 모니터링에서 더 좋은 성능을 만들기 위해 <span style='color:red'> ERF-PSPNet을 제안한다. </span> 
&nbsp;&nbsp; 이러한 모든 인지적 algorithm들은 좋은 조명 컨디션인 낮시간대의 이미지로 구성이 된다. 그러나 날씨와 조명 정도에서 변화가 생기면 이러한 성능이 떨어지게 된다. 그 이유는 semantic segmentation를 base로 하는 computer vision system들이 반대의 condition을 잘 다루지 않기 때문이다. 예를 들어, semantic segmentation은 극히 약한 조명, texture, color feature들이 과감하게 바뀌기 때문에 밤 시간 대에서는 가시 광선 카메라가 충분히 역할을 수행하지 못한다. 그러므로 Semantic segmentation의 robustness를 강화할 방법을 찾아야 한다. **이 논문에서의 목적은 밤 시간대의 semantic segmentation의 성능을 향상시키는 것이다.**
&nbsp;&nbsp; 가시광선 대신 Far-Infrared(FIR, 원적외선)을 사용하여 이러한 연구를 수행한 사람이 있다. FIR은 사용가능한 measure이지만, 해당하는 데이터셋은 오직 low-resolution image만 제공하고, 가격이 비싸 dataset이 부족하다는 단점이 있다. 반면 가시광선 데이터는 매우 싸고, 낮시간 이미지 데이터가 아주 많이 존재한다. 이러한 이유로 논문에서는 가시광선 이미지를 선택하였다. 높은 성능을 보이는 semantic segmentation은 annotated image가 많은 낮시간대의 image로 학습된 반면, 밤시간대의 annotating data를 수집하는데는 많은 시간과 노력이 필요하다. 인간의 노력으로 밤시간대의 불리한 조건에 대해 pixel을 annotated하는 작업은 사실상 불가능하다.
&nbsp;&nbsp; 이 논문에서는 낮시간에서 밤시간대로 바뀔때 발생하는 성능 저하를 극복하는 framework를 제안한다. GAN에서 영감을 받아 야간 이미지는 제안된 첫 번째 step인 전처리 단계에서 inference할 때 즉각적으로 변환된다. 다른 방식으로는 BDD 같은 datasetd에서 주간 이미지의 일부를 야간 이미지로 변환시켜 원래의 dataset을 보강한다. 

# 2. Related works

## 2.1 Semantic Understanding of the Road Scene
- SegNet

## 2.2 Model Adaption

- data anugmentation techniques : Random cropping, random rotation, flipping

## 2.3 Image Stylization

- GAN based : pix2pix, CycleGAN

# 3. Method
![Image5](/assets/images/EFFPSPNet/image1.jpg){: .align-center}

&nbsp;&nbsp; 논문에서는 semantic segmentation에서 주간과 야간 image의 gap을 줄이는 두가지 방법을 제안한다. 이러한 방법들은 상대적으로 day2night, night2day의 변환하는것과 관련이 있다. 위 그림은 논문의 framework를 나타낸다. 두 방법 모두 CycleGAN을 이용하여 domain converting을 수행했고, 첫 번째 방법에서는 image domain 이동을 위해 야간 이미지를 주간 이미지로 변환하고, 후에 **ERF-PSPNet**을 주간 이미지로 학습하여 추론을 진행하였다. 두 번째 방법에서는 CycleGAN으로 주간이미지를 야간 이미지로 변환하여 training set을 만들고 domain 확장을 진행하였다. 이후 training dataset에 특정 비율의 야간 이미지를 추가하여 ERF-PSPNet을 학습시켜 야간 이미지의 성능을 향상시켰다. 

## 3.1 Training CycleGAN for Night-day Domain Converting

&nbsp;&nbsp; 이 챕터에서의 목표는 day2night, night2day image translation을 위한 GAN을 학습시키는 것이다. Image-to-Image translation은 input, output 이미지를 정렬된 paired training set으로 만드는 것이 목표인 vision task의 한 종류이다. 그러나 논문의 task에서 큰 규모의 paired training data는 이용할 수가 없다. 이론적으로 가능한 paired dataset을 스스로 수집했음에도 불구하고, 모든 다른 style의 scene dataset을 수집하는 것은 실용적이지 않다. 그래서 필요한 것은 일반적인 야간 domain 변환이다.
&nbsp;&nbsp;