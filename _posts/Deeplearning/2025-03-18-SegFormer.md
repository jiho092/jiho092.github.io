---
layout: single
title: "SegFormer: Simple and Efficient Design for Semantic Segmentation with Transformers"


excerpt : ""
categories: 
    - deeplearningCV #카테고리설정
tag: 
    - [] #테그

date: 2025-03-18
last_modified_at: 2025-03-18
classes: #wide    
---
[SegFormer: Simple and Efficient Design for Semantic Segmentation with Transformers](https://arxiv.org/pdf/2105.15203){: .btn .btn--primary}


# Abstract

&nbsp;&nbsp; 이 논문에서는 SegFormer라는 새로운 Semantic Image segmentation Framework를 제안한다. 이 모델은 Transformers와 경량 Multi layer perceptron Decoder를 결합한 모델로, 간단하고 효율적이며 강력한 모델이다. 이 모델은 두 가지 특징을 가지고 있다.

1. hierachical structure Transformer Encoder

    &nbsp;&nbsp; SegFormer는 새로운 hierachical Transformer Encoder를 통해 multiscale feature를 출력한다. 이 모델은 positional Encoding을 필요로 하지 않기 때문에, train, test에서 해상도가 다를 때 성능 저하를 막을 수 있다.

2. SegFormer avoids complex decoders

    &nbsp;&nbsp; SegFormer는 복잡한 decoder를 사용하지 않고, 대신 MLP decoder를 사용하여 다양한 layer에서 정보를 결합한다. 이 방법은 Local Attention과 global Attention 모두를 결합하는 방법으로 강력한 representation을 생성한다. 이 효율적인 설계가 Transformer를 기반으로 하는 효율적인 segmentation의 핵심이다.

&nbsp;&nbsp; SegFormer는 SegFormer-B0부터 B5까지 여러 모델로 확장될 수 있으며, 이전 모델들보다 성능 및 효율성이 향상되었다. 예시로 SegFormer-B4는 64M 개의 parameter만 가지고도 ADE20K에서 50.3% mIoU를 보였고, 이전 SOTA 모델보다 5배 가벼우면서 성능이 2.2% 증가하였다. SegFormer-B5는 Cityscapse validation set에서 84%의 성능을 보이며, Cityscapes-C에서 Zero-shot robustness도 뛰어난 것을 확인할 수 있다.


# 1. Introduction

&nbsp;&nbsp; Semantic segmentation은 pixel에 대한 분류를 하는 task로 Classification과 밀접한 관계를 가진다. FCN(Fully Connected Networks)는 Segmentation에서 나온 중요한 방법으로, 이후 많은 연구에서 이를 기반으로 발전했고, FCN은 **dense prediction**을 위한 설계 방식이 되었다.

&nbsp;&nbsp;Classfication과 밀접한 관련이 있기 때문에, 많은 Semantic Segmentation에서는 Classification architecture가 Backborn으로 사용되었고, 중요한 연구분야로 남아있다. 초기 VGG를 시작으로 최신 방법으로 오면서 backborn은 Segmentation의 성능을 향상 시켰다.

&nbsp;&nbsp; Segmentation에서 또다른 중요한 연구 분야는 Structured prediction으로서 Segmentation을 모델링 하는 방법이다. 이 분야는 Context 정보를 효과적으로 얻어낼 수 있는 module과 연산을 포함하며, inflating으로 receptive field를 확장시키는데 효과적이다.

&nbsp;&nbsp; 자연어처리 분야에서 Transformer가 큰 성공을 하면서 Vision 분야에도 ViT model이 등장하였는데, ViT는 두 가지 한계점이 존재한다.

1. multi scale feature 대신 single scale low-resolution feature만을 사용
2. 큰 이미지에서 많은 cost가 발생

![Image5](/assets/images/SegFormer/image1.jpg){: .align-center}

&nbsp;&nbsp; 논문에서는 이러한 한계를 극복하기 위해 SegFormer라는 새로운 Transformer framework를 제안한다. 이 모델의 특징은 다음과 같다.

1. Positional encoding을 사용하지 않는 Hierarchical Transformer Encoder

    &nbsp;&nbsp; Positional Encoding을 사용하지 않아, Train 과정에서 다른 해상도를 가진 이미지가 들어와도 성능 저하가 없음

2. 경량화된 Multi Layer perceptron(MLP) Decoder

    &nbsp;&nbsp; light한 MLP Decoder를 사용하여 local attention과 global attention을 모두 처리하여 복잡한 decoder를 사용하지 않고도 좋은 성능을 보임

# 2. Related Work

## 1. Semantic Segmentation

&nbsp;&nbsp; 이미지를 pixel 단위로 구분하기 위해 FCN 방법이 많이 사용되었고 Receptive Field를 확장하거나 Contextual Information을 다루는 방법으로 발전해왔다.

## 2. Transformer Backborns

* ViT
* DeiT : distillation
* Swin Transformer, CvT, CoAt : local feature를 새건

## 3. Transformers for Specific Tasks

* DETR(Detection Transformer) : Non-Maximum suppression 없이 object detection task를 하기 위한 end-to-end framework
* SETR(Semantic Segmentation Transformer) : ViT를 backborn으로 사용하여 segmentation을 진행

&nbsp;&nbsp; 그러나 Transformer는 시간이 많이 걸려 real-time task에 부적합하다는 단점이 있음


# 3. Methods
