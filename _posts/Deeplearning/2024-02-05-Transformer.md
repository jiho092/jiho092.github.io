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

■ Encoder:


■ Decoder: