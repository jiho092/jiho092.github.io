---
layout: single
title: "Image Inpainting Survey" #제목
excerpt : ""
categories: 
    - deeplearningCV #카테고리설정
tag: 
    - [""] #테그

date: 2024-08-06
last_modified_at: 2024-08-06
#classes: wide    
---


[Deep learning for image inpainting: A survey](https://pdf.sciencedirectassets.com/272206/1-s2.0-S0031320322X00113/1-s2.0-S003132032200526X/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLWVhc3QtMSJGMEQCIDrwpc1hst113acKFtydMK0snwDAF4QGCJkm98lghEZ9AiBW9YzAK6BIZ0v%2Bc3baNzj388m9oZEdg0Uy%2BzbG6Xzd2yq8BQjs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAUaDDA1OTAwMzU0Njg2NSIM6W%2BQqpia8ZmSe%2FX0KpAFkjDPb4%2BZ%2BSkwtZyQh2B1HfJnFiJgz10dZcu99v5vCFCm7wwfgyhUcq6yb8taoRoS6%2Fzk%2FO10gNqAV9bUAdw0M2KAwwR2YyxlfAsAeOQSAam9gsnUPH%2BovoJu%2BRHSgZaOft6vXk0RHDACGtPlv%2BChGm8SamQgqJopxNx47ygIagYRQ5D4JZDb3ooDE8sg2ibBfpo1Bc9vkObeGJhUgMdwmSH6KskhWEwG7VG6T%2Fzx9JAzGpc6WeMi45z5E%2B9K6RJrNrVkvkB8pF7tctfALk%2BYAb%2Bqypmg8v%2B05Ok%2BJ2TEUkgMCrwvgi4vNsob4tzJa52ylkEdUCnNrKfF75EidZvfTraLqDXmZA4oGBa9BLJqS5J5FoQmXmQc25wHrDLp%2BhXuGpOSgon2%2BpjpTha3KZ8tf5XVUhnz9W7PZcskfvJ0rNw8vT7eXmSSSPHMYjgQlg3YGu0mckoAXr50PyUkAz0UpPCR7LPTlLUBQfGkmOFKmmrsLL%2BmHfHX%2BWpM0YVwZ2BYMlYVG2ubf%2BWsQXmP5wVgKUNlJzeIfZgKexniLbT%2BP0DCvTji4t8IKLPL8cglvYtnXcM2mmVgouv6SQ5x0HQ0lBiUqZk97FcqgYNfrvxBgq0dtl5L54wmDRODDPk%2FV%2B0TA%2BMVuE7eY7%2FXes0HRXq5yLVYNWeUt%2FB61gS6QSxcsZMHWhxi12AaW8Z%2FlD6KA%2FC2V5jVBAtl6J8nIFNUGzGszI%2FWTVkLeeQL0GQcb5OKT3VWoC5TN3NU7apFG0ZeL497%2B%2FL91F3Xk1%2Busg%2BoQvotm8LJaLS%2FMwnTWpwjP1d%2Bn57vYz2gAIU6T0oS9hyDWdg8hqnBMnTvm%2F0WE%2BVOMpwCAvwWBe6XcT12%2F8P6r8mKZ%2FUwrfjHtQY6sgFCvVsXGOQXdnpm6vJes4aPzP9lFGvcdAnTvvkyJkQk6DoQYIZ5yur5pzOJW71V2qI65f9LarqBcmtP1mzqBn0zHR07gzcCZotITGQSJtJnV5pweEYF1lgwQyx8%2BPgqmluFV3R56pPbFw0fvuQSF5KqxYiP6dRnLMLJGbtAUwtf7L4NfRIk2%2Br9xa1tJKATyJ91G7hDEz%2FBKYXvN02zCJx54eFvarlTt4n72Zs4MK2TUXIF&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240806T110741Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYSVYKZJ6V%2F20240806%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=c92592860e6818fe9517b3a830c0be1ca3b11d4215285c8a233cf739a07e4fae&hash=98a4a82c18500ea52342d3fab389d406e2f7333779aec524de3e06dd3d3cc421&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S003132032200526X&tid=spdf-e5f2d1df-70ee-4387-b88a-f3aa7719b7e2&sid=3560ab9a15d33743dc7af7f023aed0b0f8fbgxrqa&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=0b175b05070059565a&rr=8aee90846e0aaa7d&cc=kr){: .btn .btn--primary}

# 0. Intro

Image Inpainting에 대한 딥러닝 기술, 구조, 전략, 평가 metric, 실제 적용을 다룬다.

___
Image Inpainting은 손상된 이미지를 타당하게 복원하는 것이 목적이고, crack이나 object를 삭제하고, 손상된 사진을 복원할 수 있다.
이미지에서는 gray, color의 문제만 아니라 윤곽선, 텍스쳐, 문자, 이미지의 깊이 등이 중요한 요소이기 때문에 input이 매우 복잡하다. 따라서 각 이미지에 맞는 전략과 알고리즘을 사용해야한다.

딥러닝이 발전하면서 Inpainting 분야도 Deep learning이 지배하기 시작했다. 

# 1. Inpainting

![Image1](/assets/images/inpainting/survey/image1.jpg){: .align-center}

1. 전략
    * Progressive inpainting
    * Structural Information Guided Inpainting
    * Attention Based
    * convolutions-aware inpainting
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

# 2. Inpainting strategies

여기에서는 Inpainting 알고리즘 전략을 크게 5개로 나누어 확인한다.

* 전략
    * Progressive inpainting
    * Structural Information Guided Inpainting
    * Attention Based
    * convolutions-aware inpainting
    * Pluralistic Inpainting

## 2.1 Progressive inpainting

Progressive inpainting에서도 5개의 subtext로 나누어 본다.

* coarse-to-fine
* part-to-full
* low-to-high-resolution
* structure-to-content
* mask-to-image

### 2.1.1 Coarse-to-fine inpainting

![Image1](/assets/images/inpainting/survey/image2.jpg){: .align-center}

Coarse-to-fine은 Coarse Network, Refine Network 두 step이 존재하고, Coarse Network는 위 그림과 같이 단순한 encoder-decoder 구조를 가지고, Reconstruction Loss를 사용한다. 

Refine Network에서는 Encoder인 두 Network가 병렬적으로 연결되어있다. 이것이 병합되어 decoder를 통과한다. 이때 Loss는 Reconstrucntion Loss, Adversarial Loss가 있다.

### 2.1.2 Part-to-full inpainting

이 전략은 inpainting을 여러 subtask로 나누고, 각 subtask는 가장 바깥쪽부터 중심으로 가면서 복원을 수행한다. 중간 결과는 LSTM으로 공유된다.

### 2.1.3 Low-to-high-resolution inpainting

Image superresolution 방법

### 2.1.4 Structure-to-content inpainting

일반적으로 사용하는 구조는 edge이다. edge-to-content inpainting은 2-stage network architecture를 사용한다. 또한 segmentation 구조도 존재한다. 자세한 것은 reference를 참고하자.

### 2.1.5 Mask-to-image inpainting

이전의 inpainting algorithm은 missing region을 알고있고, mask로 나타낸다고 가정한다. 그러나 mask를 생성하는 것이 많은 비용이 들기 떄문에, 자동적으로 mask를 생성하는 것이 필수적이다. 

대표적으로 Wang [35]이 제안한 논문에서는 이를 위해 2-stage network architecture를 사용한다. mask prediction network의 output은 잠재적으로 image를 나타낸다.