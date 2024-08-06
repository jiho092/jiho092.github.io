---
layout: single
title: "Image Inpainting Survey" #제목
excerpt : ""
categories: 
    - Inpainting #카테고리설정
tag: 
    - ["NLP"] #테그

date: 2024-08-06
last_modified_at: 2024-08-06
#classes: wide    
---


# Intro

Image Inpainting에 대한 딥러닝 기술, 구조, 전략, 평가 metric, 실제 적용을 다룬다.

___
Image Inpainting은 손상된 이미지를 타당하게 복원하는 것이 목적이고, crack이나 object를 삭제하고, 손상된 사진을 복원할 수 있다.
이미지에서는 gray, color의 문제만 아니라 윤곽선, 텍스쳐, 문자, 이미지의 깊이 등이 중요한 요소이기 때문에 input이 매우 복잡하다. 따라서 각 이미지에 맞는 전략과 알고리즘을 사용해야한다.

딥러닝이 발전하면서 Inpainting 분야도 Deep learning이 지배하기 시작했다. 

# Inpainting

1. 전략
    * Progressive inpainting
    * Structural Information Guided Inpainting
    * Attention Based
    * Pluralistic Inpainting

2. 구조
    * Autoencoder-based
    * VAE-based
    * GAN-based
    * Diffusion-based

3. Loss
    * Reconsturction Loss (Weighted, Multi-scale...)
    * Adversarial Loss (WGAN, LSGAN, Global and Local adversarial Loss, PatchGAN)
    * Others (Markov Random, Total Variational)

