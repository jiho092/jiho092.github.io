---
layout: single
title: "신경망 구조" #제목
excerpt : ""
categories: 
    - deeplearningbasic #카테고리설정
tag: 
    - ["deeplearning"] #테그

date: 2024-01-11
last_modified_at: 2024-01-11
classes: wide    
---


# 퍼셉트론



input과 weight를 곱한 합을 activation function을 통과시켜 output을 생성한다.



$y=f(wx+b)$



$x : $ input

$y : $ output

$w : $ weight

$b : $  bias(절편)

$f() : $  activation function


# activation function (활성함수)



1.sigmoid

$y = \frac{1}{1+e^{-x}}$



출력층에서 많이 사용한다.



2.tanh

$$
f(x) = tanh(x) = \frac{e^{x}+e^{-x}}{e^{x}+e^{-x}}
$$



3.ReLU

$$
f(x) = max(0,x)
$$



gradient vanishing 문제를 해결 but 0이 되면 ReLU가 죽음 → 다양한 ReLU로 변형(Leaky ReLU)



4.softmax

$$
softmax(x_{i}) = \frac{e^{x_{i}}}{ \sum_f^k e^{x_{i}}}
$$



분류 작업에서 많이 사용한다.



# Loss function (손실함수)



예측값과 실제값의 비교를 통해 값을 낮게 만들어 모델의 성능을 끌어 올린다.



다양한 Loss function이 존재 - pytorch 제공 Loss

■ MSE(Mean squared Error) : 회귀 문제

■ MAE(Mean Absolute Error) : 회귀 문제

■ Cross-entropy : Category 문제


# Optimizer 



딥러닝을 학습할 때 올바른 방향으로 학습해야한다. 즉, Loss function의 값을 최소화 하는 알고리즘이 Optimizer이다.



■ SGD

■ Adagrad (많이 사용)

■ Momentum

■ Adam (많이 사용)

■ NAG

■ Adam, Adagrad가 요즘 많이 선호되지만, 여러 Optimizer를 시도해보는 것이 중요하다.

(추가 공부 필요)



# batch



데이터를 분할하는 것



■ batch size : gradient를 학습하는 크기, 모델을 1회 학습할 데이터의 크기, batch size가 커지면 batch의 수는 줄어든다.

■ mini batch :한 번에 처리하는 데이터 수를 의미한다.


# 신경망 구조 학습 과정



1.데이터를 가져온다.

2.그레디언트를 초기화한 후 모델의 forward 연산을 수행한다.

3.Loss를 계산한다

4.Loss를 역전파 한 후 옵티마이저를 통해 parameter를 업데이트한다

수정 확인