---
layout: single
title: "단어의 벡터 표현(Text Vectorization)" #제목
excerpt : ""
categories: 
    - deeplearningNLP #카테고리설정
tag: 
    - ["NLP","Text"] #테그

date: 2024-01-19
last_modified_at: 2024-01-19
classes: wide    
---


# Text Vectorization을 하는 이유
 컴퓨터는 연산을 수행할 때, 0과 1 이진법을 통해서만 계산이 가능하다. 따라서 자연어처리 분야에서는 우리가 사용하는 단어를 컴퓨터가 이해할 수 있도록 문자를 수치화 하는 과정이 있어야 한다.

 단어를 벡터화 하는 방법은 크게 세가지가 존재한다 첫 번째는 One-hot Encoding이고, 두 번째는 Word Embedding, 세 번째는 TF-IDF（빈도수 기반 텍스트 벡터화）이다.  이 세가지 방법에 대해 알아보자.
 
# 1. One-hot Encoding
 단어를 벡터화하는 전통적인 방법이다. 우리가 사용하는 어휘 사전에 N개의 단어가 존재한다고 가정하면 One-hot Encoding의 벡터의 크기는 N이다. 이 방법은 특정 단어를 나타내는 한 개의 위치를 1로 지정하고 나머지는 0으로 표현한다.
 
 One-hot Encoding을 사용하면 단어를 수치적으로 표현할 수 있지만, vector가 sparse하기 때문에 공간 낭비가 심하고, 단어 사이의 관계나 의미를 표현하지 못한다는 단점이 존재한다. 즉 효율적이지가 않다.
 
 
# 2. Word Embedding
 분산 표상（Distributedsimilarity based representation）을 통해 단어를 표현한다.

 단어가 벡터 공간상에 존재하고, 비슷한 의미를 가지는 단어는 가까운 위치에 존재한다고 생각하면 좋다. 이 경우 벡터가 단어의 의미를 담고 있으므로, 단어 간의 벡터 연산이 가능하고, 또한 의미를 통해 벡터를 정의하므로 어휘 사전에 N개의 단어가 있을 때, 그보다 작은 벡터의 크기로 단어들을 정리할 수 있다.
 
 이 때 벡터로 표현하는 방법은 Neural Network를 통해 학습을 하고, 대표적으로는 Word2Vec 방법이 있다. Word2vec 학습 방법에는 Skip-gram, CBOW, 스탠포드의 GLoVe, 페이스북의 FastText가 있다.
 
 직접 학습시키기 보다는 존재하는 모델들을 주로 사용한다.
 
 
# 3. TF-IDF（빈도수 기반 텍스트 벡터화） 
            


