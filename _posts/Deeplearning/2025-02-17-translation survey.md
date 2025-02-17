---
layout: single
title: "Comparison and Analysis of Image-to-Image Generative Adversarial Networks: A Survey"

excerpt : ""
categories: 
    - deeplearningCV #카테고리설정
tag: 
    - [] #테그

date: 2025-02-17
last_modified_at: 2025-02-17
classes: #wide    
---
[Comparison and Analysis of Image-to-Image Generative Adversarial Networks: A Survey](https://arxiv.org/pdf/2112.12625){: .btn .btn--primary}


# Abstract

&nbsp;&nbsp; 이 논문은 Image-to-Image translation task 중 GAN model에 대한 survey이다. 여기서 다루는 모델은 **Pix2Pix, CycleGAN, CoGAN, StarGAN, MUNIT, StarGAN2, DA-GAN, Self-Attention GAN**이며, Image translation에서 SOTA를 달성했던 모델들이다. 또한 18개의 데이터셋과 9개의 평가 metric을 소개한다.

# 1. Introduction

&nbsp;&nbsp; GAN은 high-quality fake image를 생성하며, generator와 discriminator 두 가지 network를 사용하여 학습된다. 논문에서는 model architecture, loss function, dataset, metric에 대해 다룰 것이다. Image-to-Image GANs은 초현실적인 image를 생성하는 데에도 혁신을 가져왔다. random data의 분포를 input으로 사용하는 대신, 다른 image를 사용하여 이미지를 생성한다. 이 방법은 Translation외에도 style transfer, colorizing, inpainting, super resolution, future state prediction, object transfiguration, photo editing and enhancement 등 다양한 task에 적용될수 있다. Pix2Pix는 GAN의 최초 모델 중 하나이며, 충분히 학습하여 model 변경 없이 다양한 domain에 적용할 수 있다.

## GAN Models

1. **Pix2Pix** : GAN의 첫번째 논문이며, loss function 변경 없이 다양한 dataset에 일반화 가능
2. **CycleGAN** : paired data 필요성의 제한을 극복
3. **CoGAN** : joint image distribution을 학습하기 위해 weight sharing 도입
4. **StarGAN** : 2개 이상의 class를 단일 모델로 mapping하고 모든 class에서 공통적인 feature를 학습할 수 있게 함
5. **MUNIT** : multi-modal을 도입하여 다양한 style로 변환 가능하게 함
6. **StarGAN2** : StarGAN, MUNIT을 확장 시킴
7. **DA-GAN** : attention 을 사용하여 high quality의 image를 만든 최초의 논문
8. **Img2Img SA** : self-attention 도입으로 image translation에서 SOTA를 달성한 모델

## Model 분석

1. Architecture
2. loss function
3. metric
4. datasets