---
layout: single
title: "Attention is all you need(Transformer) 리뷰" #제목
excerpt : ""
categories: 
    - deeplearningNLP #카테고리설정
tag: 
    - ["NLP","Text","Attention","Transformer","paper"] #테그

date: 2024-02-05
last_modified_at: 2024-02-05
classes: wide    
---

자연어 처리에서 중요한 모델인 Transformer(NIPS 2017) 논문을 리뷰하려한다.

[Transformer 논문](https://arxiv.org/pdf/1706.03762.pdf/){: .btn .btn--primary}

![Transformer](/assets/images/Transformer/paper.png){: .align-center}


# 0. Abstract

기존에는 sequence 데이터를 처리할 때, encoder, decoder에 RNN과 CNN 기법을 사용하여 처리하였다. 이후 Attention mechanism을 함께 사용하며 성능이 향상되었다.

이 논문에서는 RNN, CNN을 사용하지 않고 전적으로 attention mechanism만을 사용하는 Transformer 모델을 제안하였다.

이 모델은 병렬적 처리가 가능하여 수행 시간이 매우 감소하였고, 성능이 향상되었다.


# 1. Introduction

RNN, LSTM, GRU 등의 모델은 sequence에 포함되어 있는 token 위치정보를 먼저 정렬한 후 $h_t$를 반복적으로 갱신하여 계산을 수행하였다. 따라서 병렬처리가 어려웠고, memory적으로도 비효율적이었다.

이후 Attention을 사용하여 어떤 단어가 중요한지 weight를 부여하여 보안을 해왔지만, 여전히 RNN 기반 모델들과 함께 사용되었다. 이 논문에서는 오로지 Attention 기법만 사용하여 연산을 수행하였다.

# 2. Background

생략

# 3. Model Architecture

Transformer는 기본적으로 Encoder-Decoder 구조를 가지고 있다.

![Transformer_model](/assets/images/Transformer/transformer_model.png){: .align-center}

Transformer모델은 그림에서 보는 것처럼 RNN 구조를 사용하지 않는다. 따라서 단어의 순서를 알려주기 위해 positional encoding을 사용한다. 이 방법은 이후 논문에서도 많이 쓰인다.

또한 Encoder, Decoder 모두 N개의 Layer가 반복되는 구조임을 확인할 수 있다. 이제 Encoder, Decoder 구조에 대해 알아보자

## 3.1 Encoder and Decoder Stacks


![encoder-decoder](/assets/images/Transformer/encoder-decoder.png){: .align-center}


■ Encoder:

논문에서는 Layer의 수가 6개로 나와있다. 이것은 최적의 수가 아닌 논문에서 임의로 설정한 Layer의 수 이다.

또한 Multi-Head Attention에 들어가는 세가지 방향의 화살표는 input에서 뽑아낸 Query,Key, Value를 의미한다.

### 1. input Embedding

![input](/assets/images/Transformer/input.png){: .align-center}

먼저 자연어를 컴퓨터로 연산하기 위해 input Embedding 과정을 거친다. 이 과정은 위 이미지와 같은 방법으로 수행되는 것을 알 수 있고, 블로그에 방법을 리뷰해 놓았으니 참고하길 바란다.

### 2. Positional Encoding

Attention만을 사용하면 순차적으로 연산을 수행하는 것이 아니기 때문에, token을 처리할 때 단어의 순서정보를 알 수 없다. 따라서 단어의 순서 정보를 입력해주기 위해 Positional Encoding 과정이 필요하다.

positional Encoding으로는 주로 다음과 같은 식을 사용한다.

$PE_{(pos,2)} = sin(pos/10000^{2i/d_{model}})$

$PE_{(pos,2i+1)} = cos(pos/10000^{2i/d_{model}})$

해당 positional embedding 값과 input embedding 값을 elemental_wise하여 위치의 정보를 더하여 연산을 수행한다.

### 3. Multi-head attention

각각의 단어가 서로에게 연산하여 Attention score를 구하고 단어들의 연관성을 구한다.

$Attention(Q,K,V) = softmax(\frac{QK^T}{\sqrt{d_k}})V$

$MultiHead(Q,K,V) = Concat(head_1,head_2,...,head_h)W^O$

$head_i = Attention(QW_i^Q,KW_i^K,VW_i^V)$

### 4. Residual Learning(잔여학습)

Residual Learning은 그림에서 왼쪽으로 나가서 Multi-Head Attention을 거치치 않고 그대로 넘어가는 것을 의미한다. 

이 방법을 사용하면 기존 정보를 입력 받으면서 잔여 부분만 학습되도록하여 모델 수렴속도가 높아지고 Global Optim을 찾으면서 모델 성능이 올라가게 된다.

위 4개의 과정이 Encoder Layer 하나의 과정이고 Layer만큼 반복하여 연산을 수행한다. 여기서 Layer들의 parameter는 각각 다르다.

마지막 Layer에서 출력된 값이 최종적으로 모든 Decoder Layer에 들어가게 된다. Decoder에서는 <eos>가 등장할 때까지 연산을 수행하고 Context vector로 압축하는 과정이 사라져서 Bottle neck 현상을 방지할 수 있었다.


■ Decoder: