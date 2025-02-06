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
&nbsp;&nbsp; CycleGAN은 paired data가 부재할 때, source domain에서 target domain으로 이미지를 변환을 위해 학습하는 접근법이다. CycleGAN은 2개의 GANs Set을 포함하고 있고, 각각 generator와 discriminator를 포함하고 있다. generator와 discriminator는 domain $X$에서 $Y$로 혹은 그 반대로 이미지를 변환 시켜 주고, 2개의 GANs은 $F$, $G$ 2개의 Generator가 있으며 서로 반대 방향이고, 두 generator를 동시에 학습시킨다. loss는 다음과 같다. 이 loss는 unpaired image-to-image translation을 가능하게 만들어준다.

$$
F(G(x)) \approx x
$$

$$
G(F(y)) \approx y
$$

데이터는 **BDD100K** dataset으로 부터 6000개의 주간 이미지와 이미지를 수집하여 CycleGan을 훈련시켰고, 이미지는 480X270으로 resize하여 day2night, night2day converter를 얻어냈다.
## 3.2 Converting Image to the Daytime During Inference
&nbsp;&nbsp; 첫 번째 방법은 야간 이미지를 주간 이미지로 변환하는 것이다. 특히, 카메라로 수집한 야간 이미지를 semantic segmentation에 사용 가능한 domain인 주간 이미지로 변환한다. 
&nbsp;&nbsp; 이 방법은 모델을 재학습 시킬 필요가 없고, 안정적인 데이터로 학습된 ERF-PSPNet의 원래 학습된 weight를 그대로 사용할 수 있다. 추가적으로 night-day 변환과 semantic segmentation 추론을 분리할 수 있다. 그러나 inference 과정의 computational cost가 증가하게 되고 이것은 real-time semantic segmentation에서 단점이 될 수 있다. 각 추론 과정에서 CycleGAN으로 변환된 이미지는 ERF-PSPNet에 먹여진다. 효율적인 segmentation network가 정확한 segmentation map을 예측하는데 쉽게 사용될 수 있지만, 480X270 image에서 CycleGAN의 정방향은 거의 1초가 소요된다. 이것은 매우 느린 속도이며, real-time에서 기대하기 어렵다. 또 다른 단점은 original image와 비교하여 GAN으로 만든 image가 bias가 존재한다는 것이다. 
## 3.3 Gernerating Nighttime Images to Expand the Training Set

&nbsp;&nbsp; 두 번째 방법은 **BDD10K** 주간 이미지를 segmentation label과 함께 야간이미지로 변환시키는 것이다. 이 때, 생성된 야간 이미지 training set은 ERF-PSPNet에 넣어진다. 이 아이디어는 nighttime dataset의 명확한 라벨이 없다는 점에서 나왔다.
&nbsp;&nbsp; 이 방법의 장점은 trained model의 inference 과정에서 추가적인 계산이 필요 없다는 것이다. 이로 인해 real-time property를 유지할 수 있다. 논문의 실험에서 night image 생성 비율이 semantic segmenetation 정확도에 얼마나 영향을 주는지 탐구하였고, 실험을 바탕으로 training dataset에 필수적인 night image를 추가하였다.
&nbsp;&nbsp; 그러나 이 방법의 단점은 모델을 재학습시킬 때, 시간 소모가 크다는 점과 모든 종류의 환경에서 robust하지 않을 수 있다는 점이다. 추가적으로 CycleGAN은 parameters의 크기가 거대하기 때문에, 원본 사이즈가 아닌 480X270으로 resize하여 학습을 시켰다. 이러한 이유로 GAN으로 오직 480X270 이미지만 사용 가능하고, 최종 예측 정확도에 영향을 주는 것을 피할 수 없었다.

# 4. Experiments.

## 4.1 Datasets and Training


![Image5](/assets/images/EFFPSPNet/image3.jpg){: .align-center}

&nbsp;&nbsp; 실험의 첫 번째 단계는 야간이미지에서 주간이미지로 변환하고 반대의 경우로도 변환한 것으로 GAN을 학습시키는 것이다. 이 때, CycleGAN의 데이터셋은 image가 pair일 필요는 없다. 그러나 dataset은 주간, 야간 상황에서 다양한 driving image를 반드시 포함해야한다. Cityscapes, Mapillary Vistas와 같은 대부분의 mainstream dataset에서 night time image가 부족하다. 논문에서는 최근에 개방된 BDD dataset을 사용한다.
&nbsp;&nbsp; **BBD** dataset은 **BDD100K, BDD10K**로 이루어져 있다. 전자는 다양한 condition, 시간에서 10,0000장의 driving image로 이루어져 있으며, GPS, OBD annotator, 차선과 주행 가능한 지역의 annotation 정보가 담겨있다. 후자는 9,000개의 pixel단위로 annotator가 달린 image가 있고, 19개의 label을 가진 1,000개의 test image가 존재한다.
&nbsp;&nbsp; 실험을 real world에서 적용시킬 수 있는지 정량적으로 검증하기 위해, **Yuquan Campus of Zhejiang University**에서 **Multi-Modal Stereo Vision Sensor**를 이용하여 주, 야간 이미지를 수집하였다. 1,500개 정도의 이미지를 수집했고, Nighttime Driving Test dataset을 활용하였다. 위 테이블과 그림에 자세한 정보가 나와있다.

![Image5](/assets/images/EFFPSPNet/image2.jpg){: .align-center}

* 사용 데이터 정리
1. BDD Dataset
2. ZJU Dataset
3. Nighttime Driving Dataset

&nbsp;&nbsp; BDD Dataset과 ZJU Dataset의 style의 차이가 크기 때문에, 실험을 할 때 두가지 데이터 셋을 각각 학습시켰다. BDD100K에서는 모두 맑은 날씨에서 6,000개의 주간이미지와 야간이미지를 선택하였고, 이 데이터로 이미지 변환을 학습시킨 것을 BDD-GAN이라 하고, ZJU dataset 850개의 day-time image pair 학습한 모델을 ZJU-GAN이라 부른다.
&nbsp;&nbsp; 대부분의 semantic segmentation model처럼 **ERF-PSPNet** 또한 Encoder-Decoder로 구성된다. Encoder는 ImageNet에서 학습된 것을 가져왔으며 ERF-PSPNet은 Decoder를 훈련할 때 사용한다. 첫 번째 방법에서 모델은 BDD10K로 학습되었고, inference 과정에서 실시간으로 야간이미지를 주간이미지로 변환시켰다. 두 번째 방법에서는 BDD10K의 이미지를 다른 비율로 사용하여 모델을 학습시켰다. 정량적 검증을 위해 BDD10K test set에서 32개의 night image와 Nighttime Driving Test Dataset에서 50개의 night image를 가져왔다. 두 데이터 셋은 이미지 스타일이 비슷하여 reasonable한 결과를 만들 수 있다.

## 4.2 Quanlitative Results


![Image5](/assets/images/EFFPSPNet/image4.jpg){: .align-center}

* BDD Dataset에서의 결과

    - 주간, 야간, 주간 변환 이미지 순
    - 우측 그림에서는 큰 효과를 보이는 것을 알 수 있음

![Image5](/assets/images/EFFPSPNet/image5.jpg){: .align-center}

* ZJU Dataset에서의 결과

    - 주간, 야간, 주간 변환 순
    - 어둠때문에 잘 인식되지 않은 나무, 자동차 들에 대해 개선된 것을 볼 수 있음

## 4.3 Quantitative Results and Discussion

![Image5](/assets/images/EFFPSPNet/image6.jpg){: .align-center}

&nbsp;&nbsp; 두 가지 dataset에 대해 정량적인 평가 지표를 정리한 테이블이다. 첫 번째 방법에서는 baseline보다 수치가 낮은 결과가 나왔지만, 하늘, 차, 트럭과 같은 class에서는 정확도가 유의미하게 향상되었다. 수치가 낮게 나온 이유는 CycleGAN이 생성한 이미지의 texture가 detail하지 않기 때문이라고 판단한다. 또 다른 이유는 밤이기 때문에 BDD dataset과 Nighttime dataset은 조명이 적절하게 있기 때문에, 이 방법이 baseline보다 좋기는 어렵다.
&nbsp;&nbsp; 두 번째 방법의 결과는 아래 4개의 행이다. MIOU의 경우에는 유의미하게 정확도가 향상되었고 이전에 제안된 방법인 (32)의 방법보다 약 4% 증가한 것을 볼 수 있다. 

![Image5](/assets/images/EFFPSPNet/image7.jpg){: .align-center}

&nbsp;&nbsp; 또한 야간 이미지의 비율을 찾는 중요한 실험을 진행하였고 결과는 아래와 같다. 0-2000개의 범위에서 야간 이미지 탐지의 성능이 향상되었고, 이후에는 감소하다가 7000개에서 크게 감소하는 것을 확인할 수 있다.

![Image5](/assets/images/EFFPSPNet/image8.jpg){: .align-center}

# 5. Conclusions
&nbsp;&nbsp; 이 논문에서는 nighttime 이미지에서 semantic segmentation 모델을 사용할 때 발생하는 문제에 대해 연구하였고, 성능을 향상시키기 위해 CycleGAN을 이용한 2가지 방법을 제안하였다. 또한 성능을 검증하기 위해 정량적, 정성적 평가를 진행하였으며 포괄적인 실험 setting이 path와 train을 결정하는데 적합한 장소를 나타낸다. 결론적으로 ERF-PSPNet이 밤에도 좋은 성능을 보임을 알 수 있다.
