---
layout: single
title: "NExploring Reliable Matching with Phase Enhancement for Night-time Semantic Segmentation"


excerpt : ""
categories: 
    - deeplearningCV #카테고리설정
tag: 
    - [] #테그

date: 2025-03-26
last_modified_at: 2025-03-26
classes: #wide    
---
[Exploring Reliable Matching with Phase Enhancement for Night-time Semantic Segmentation](https://arxiv.org/pdf/2408.13838){: .btn .btn--primary}


# Abstract

야간 image semantic segmentation은 CV에서 중요한 문제로 자율주행에서 중요한 역할을 한다. 그러나 기존 방법들은 주간 이미지 관점에서 야간 이미지를 해석하는 경향이 있어, 저조도 환경에서의 본질적인 문제(texture 왜곡, matching 오류)를 해결하지 못했다. 이 문제를 해결하기 위해 논문에서는 **NightFormer**라는 새로운 end-to-end optimized approch를 제안한다. 이 방법은 야간 image의 특성을 반영하고, 주간 이미지로 강제로 맞추는 기존 방법을 피한다. 구체적으로 논문에서는 texture인식을 위한 **pixel-level texture enhancement module**과 저조도 환경에서 정확한 matching을 실현하는 **object-level reliable matching module**을 설계했다. 다양한 benchmark dataset에서 좋은 성능을 보였다.

# 1. Introduction
![Image5](/assets/images/NightFormer/image1.jpg){: .align-center}
Semantic Segmentation은 중요한 CV task지만 야간 이미지에 대해 한계가 있기 때문에, End-to-End NightFormer를 제안한다.
이 방법은 pixel-level texture enhancement module과 object-level reliable matching module을 통해서 야간 이미지를 주간 이미지에 맞추는 방법을 피한다.

# 2. Related work

## 2.1 Semantic Segmentation

## 2.2 Night-time Semantic Segmentation

* NightCity
* Nightlab
* DTP

# 3. Method

## 3.1 Overview

![Image5](/assets/images/NightFormer/image2.jpg){: .align-center}

위 그림같이 input image $x \in R^{H \times W \times 3}$이 주어지면, pixel level texture enhancement module은 **image encoder**와 **Fourier 기반 Encoder**를 이용하여 **hierarchical pexel embedding**과 **texture aware feature**를 추출한다.

이후 Hierarchical amplified decoder는 multi scale에서의 특징을 증폭시켜 은밀한 object(inconspicuous objects)들을 탐색한다. 

enhancement features $F^a$는  object-level reliable matching module로 전달되어 Semantic aware prototype과의 상호작용을 통해 정확한 인식을 가능하게 하고, Semantic correction features을 강화하여 최종 segmentation output을 생성하게 된다.

## 3.2 Pixel-level Texture Enhancement module

pixel level에서 texture와 target information을 구분하기 위해 논문에서는 Fourier frequency domain을 사용하여 texture 세부 표현을 먼저 모델링한다. 이후 hierarchcal amplified decoder를 통해 texture information을 증폭하여 pixel level 특징을 향상시킨다.

### Phase Texture Extraction

기존 Fourier Transform을 활용한 방법들은 주로 연산 효율, style transfer에 집중했지만, night-time image에서는 손상된 texture를 복원하기 위한 Fourier phase component의 잠재력에 주목하고 있다.

Fourier spectrum의 phase information은 이미지 구조에 대한 high-level statistic information을 포함하고 있고, 이를 통해 흐릿한 경계나 손상된 detail을 복원한다.

* 야간이미지 $x$에서 2D Fourier 변환

$$
\mathcal{F}(x)_{u,v} = \sum_{j=0}^{H-1} \sum_{i=0}^{W-1} x_{i,j,c} e^{-j2\pi\left(\frac{ui}{H} + \frac{vj}{W}\right)}
$$


* 복소수 $F(x)_{u,v}$의 amplitude(진폭), phase(위상)은 다음과 같다.
$$
\mathcal{A}(x)_{u,v} = |\mathcal{F}(x)_{u,v}|
$$

$$
\Phi(x)_{u,v} = \arg(\mathcal{F}(x)_{u,v}) = \arctan\left(\frac{\text{Im}(\mathcal{F}(x)_{u,v})}{\text{Re}(\mathcal{F}(x)_{u,v})}\right)
$$

* amplitude는 평균값 $C^a$로 고정하며 inverese Fourier transform을 통해 phase-only feature map $\Phi(x)$를 생성한다.


$$
\bar{\Phi}(x) = \mathcal{F}^{-1}[c^a \cdot e^{j\Phi}]
$$

* 이 pahse characteristic은 가벼운 encoder(ResNet-18)을 통해 encoding되어 이후 decoder 증폭 모듈에 전달된다.


### hierarchical Amplified Decoder

보다 정밀한 target information을 추출하기 위해 backborn으로 부터 $F_5,F_4,F_3,F_2$의 다양한 level의 feature를 먼저 추출하고, 이후 각 단계에서 hierarchical amplified mechanism을 통해 대응되는 amplified pixel-level features를 생성한다.

* amplified map $A$

$$
A_{i,j} = \sum_{c=1}^{C} (F_{i,j,c} + \bar{\phi}_{i,j,c})^2
$$

* A를 original image feature $F$에 곱하여 최종 amplified feature $F^a$생성

$$
F^a = F \circ A
$$

$\circ$  : Element wise product

또한, self-attention 계층을 사용하여 픽셀간의 특징을 추가적으로 집계하고 upSampling 단계를 통해서 최종 고해상도 특징 $E$를 생성한다.

## 3.3 Objet-level Reliable Matching Module

다양한 의미를 가진 target information을 효과적으로 합치기 위해 논문에서는 다음과 같은 prototype set을 학습한다
$P = {p_n}_{n=1}^N$, 여기서 $p_n \in R^{1 \times L}, N$ : prototype의 수

프로토타입은 학습 가능한 query vector로서 attention을 통해 동적인 방식으로 class간의 지식을 습득한다.
