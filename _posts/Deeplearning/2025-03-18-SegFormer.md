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
![Image5](/assets/images/SegFormer/image2.jpg){: .align-center}

&nbsp;&nbsp; SegFormer는 위 그림처럼 두가지 주요 module로 구성된다.

1. Hierarchical Transformer Encoder : high, low resolution 특성을 생성

2. Lightweight All-MLP Decoder : multi level feature를 합쳐 최종적인 mask 예측


&nbsp;&nbsp;  **H × W × 3** 크기의 이미지를 입력받으면, 먼저 이를 **4 × 4** 크기의 패치로 분할한다. **ViT**가 **16 × 16** 크기의 패치를 사용하는 것과 달리, 작은 패치가 **dense prediction task**에 유리하기 때문에, **SegFormer**는 이 패치를 **계층적 Transformer Encoder**에 입력하여 고해상도 및 저해상도 feature들을 얻는다. 그 후, **multi level feature(original의 1/4, 1/8, 1/16, 1/32)**들을 **All-MLP Decoder**에 전달하여 **$H / 4 × W / 4 × N_{cls}$** 해상도에서 segmentation mask를 예측한다. 

여기서 $N_{cls}$는 카테고리 수


## 3.1 Hierarchical Transformer Encoder

&nbsp;&nbsp; 논문에서는 Mix Transformer(MiT) architecture Series를 디자인한다.(MiT-B0 ~ MiT-B5) 이 모델들의 차이는 같은 architecture를 가지지만, size가 다르다. MiT-B0는 경량 모델, MiT-B5는 성능을 위한 모델이다. 이 디자인은 ViT에서 영감을 받아 Segmentation에 최적화 되었다.



### 1. Hierarchical Feature Representation

![Image5](/assets/images/SegFormer/image3.jpg){: .align-center}

&nbsp;&nbsp; **Single-resolution feature mape을 받는 ViT** 와 달리 **SegFormer에서는 CNN과 유사하게 Image patch를 입력으로 받아 Multi-level의 feature를 생성**하는 Hierarchical Transformer 구조를 사용한다.

&nbsp;&nbsp; 구체적으로 보면 다음과 같다.

* input image size : $H \times W \times 3$의 형태태

* $\frac{H}{2^{t+1}} \times \frac{W}{2^{t+1}} \times C_i$ 의 형태로 나누어 Hierarchical Feature 생성 $i \in {1,2,3,4}$, $C_i : i번째 계층의 채널 수$ 

* $F_i$ : $i$번째 계층의 feature map

* patch merging을 통해 low resolution의 feature와 high resolution의 feature를 생성


### 2. Overlapped Patch Merging

&nbsp;&nbsp; ViT와 유사하게 Image patch가 주어졌을 때의 patch merging 방식은 다음과 같다.

1. $N \times N \times 3$크기의 패치를 $1 \times 1 \times C$차원의 벡터로 변환

2. $1 \times 1 \times C$로 patch를 통합하여 hierarchical featur map을 얻음

3. 이를 통해 $F_1(\frac{H}{4} \times \frac{W}{4} \times C_1)$ 을 $F_2(\frac{H}{8} \times \frac{W}{8} \times C_2)$로 hierarchical featur를 축소할 수 있음

&nbsp;&nbsp; 이 과정은 처음에는 겹치지 않는 image 또는 feature patch를 결합하기 위해 설계되었다. 그러나 이러면 local conitunity를 유지할 수 없기 때문에 Overlapping patch merging을 사용하여 겹치게 설계하였다. 이를 위해 $K,S,P$를 정의한다.

* $K$ : patch size
* $S$ : stride
* $P$ : padding size

&nbsp;&nbsp;Experiment에서는 $K=7, S=4, P=3$으로 설정하였고, 비교를 위해 $K=3, S=2, P=1$을 사용하여 non overlapping과 동일한 size의 feature를 생성하도록 하였다.


### 3. Efficient Self Attention

&nbsp;&nbsp; Self Attention은 Transformer Encoder의 주요 computation bottleneck이다. 원래의 Self Attention에서는 $Q, K, V$의 차원이 동일하고 Sequence의 길이 $N$에 따라 계산 복잡도가 $O(N^2)$이었다. 

$$
\text{Attention}(Q, K, V) = \text{Softmax} \left( \frac{QK^T}{\sqrt{d_{\text{head}}}} \right) V
$$

&nbsp;&nbsp;이를 해결하기 위해 Self Attention의 Sequence를 축소하였다. 

* $K$ : 줄인 Sequence의 길이
* **Reshape** : $K$의 차원을 $\frac{N}{R} × (C \cdot R)$으로 변환
* **Linear($C_{in}$, $C_{out}$)($\cdot$)** : $C_{in}$의 dimensional tensor를 받아 $C_{out}$ 차원의 tensor를 출력


$$
\hat{K} = \text{Reshape} \left( \frac{N}{R}, C \cdot R \right)(K)
$$

$$
K = \text{Linear}(C \cdot R, C)(\hat{K})
$$

이 방법을 통해서 시간 복잡도가 $O(N^2)$에서 $O(\frac{N^2}{R})$까지 줄어들었다. 실험에서는 $R$을 [64,16,4,1]로 설정하여 stage1에서 stage4까지 적용시켰다.

### 4. Mix FFN

&nbsp;&nbsp; ViT에서는 Positional Encoding을 사용하는데, 해상도가 고정되기 때문에 다른 해상도에서는 성능이 떨어질 수 있다. 이 문제를 해결하기 위해 **CPVT**는 3X3 Convolution과 positional Encoding을 함께 사용하여 구현하였다.

&nbsp;&nbsp; 그러나 논문에서는 Positional Encoding이 Semantic Segmentation에서 필요없다고 주장한다. 대신 Mix-FFN을 도입하여 Zero padding이 위치 정보의 약점을 보완해준다. 이 과정은 다음 수식과 같다.

$$
x_{\text{out}} = \text{MLP}\left( \text{GELU}\left(\text{Conv3x3}\left(\text{MLP}(x_{\text{in}})\right)\right)\right) + x_{\text{in}}
$$

* $x_{in}$ :: self attention module에서 나온 feature map

&nbsp;&nbsp; Mix-FFN은 3 × 3 Convolution과 MLP를 결합하여 각 FFN에 위치 정보를 제공한다. 실험에서는 3 × 3 Convolution이 Transformer에 대한 위치 정보를 제공하는 데 충분하다는 것을 보일 것이다. 특히, 우리는 convolution depth-wise 3 × 3 Convolution을 사용하여 parameter 수를 줄이고 효율성을 향상시킨다.



## 3.2 Lightweight All-MLP Decoder

&nbsp;&nbsp; SegFormer에서는 오직 MLP만 포함하는 Lightweight Decoder를 사용하며, 다른 방법들과 달리 수작업으로 만들어진 계산량이 많은 복잡한 component를 피한다. 이것이 가능한 이유는 CNN보다 Transformer Encoder가 Effective Receptive Field(ERF : 효과적 수용 공간)을 가지기 때문이다.
