---
layout: single
title: "Autoencoder, Variational Autoencoder 리뷰" #제목
categories: 
    - deeplearningbasic #카테고리설정
tag: 
    - ["논문","AutoEncoder"] #테그
---

Autoencoder, VAE 논문 리뷰


# 들어가면서

이 글은 아래 두 논문을 읽으면서 공부한 내용입니다.
 
[Autoencoders - Dor Bank, Noam Koenigstein, Raja Giryes](https://arxiv.org/pdf/2003.05991.pdf){: .btn .btn--info}
<br>

[Auto-Encoding Variational Bayes - Diederik P. Kingma, Max Welling](https://arxiv.org/pdf/1312.6114.pdf){: .btn .btn--info}
<br>


# 1. Information needed to understand the paper

해당 논문을 이해하기 위해 먼저 2가지 지식이 필요했는데 이에 대해 먼저 알아보자.

## Kullback - Leibler Divergence(KLD)

![page1](/assets/images/introduction of autoencoder/슬라이드2.PNG){: .align-center}

먼저 Kullback - Leibler Divergence이다. 해당 방법은 두 개의 분포가 얼마나 다른지에 대한 정도를 나타내는 방법인데 해당 값은 분포의 차이이기때문에 값이 항상 0 이상이고, 특징 2번이 다르므로 두 분포의 거리가 아닌 두 분포가 얼마나 다른지를 나타낸 정도이다.


## Manifold hypothesis

![](/assets/images/introduction of autoencoder/슬라이드3.PNG){: .align-center}

두 번쨰는 manifold hypothesis이다. 위 가설은 고차원 데이터라도, 실질적으로 데이터를 나타내는 저차원 공간 manifold가 존재한다는 가설이다. 이 공간을 찾아내는 것이 autoencoder의 목적이라고 볼 수 있다.

# 2. Introduction to Autoencoder

지금부터 Autoencoder에 대해 본격적으로 들어가보겠다.

## Autoencoder

![](/assets/images/introduction of autoencoder/슬라이드4.PNG){: .align-center}

Autoencoder란 원본 데이터를 넣어 Encoding 과정을 통해 잠재 변수로 변환한 후 다시 Decoding 과정을 통해 재구성하는 딥러닝 학습 방법이다.

Autoencoder의 사용 목적은 데이터의 informative representation을 unsupervised 방법을 통해
학습하는 것이다. 즉, Encoding 과정을 통해 latent variable을 찾는것이 목적이다.

Autoencoder의 학습 방법은 위 식과 같다. 여기서 A,B는 각각 encoder 과정과
decoder 과정을 의미하고, input데이터 X와 input데이터가 encoder와 decoder를
통과해서 나온 output의 차이가 최소화 되도록 학습을 진행한다.

## PCA
![](/assets/images/introduction of autoencoder/슬라이드5.PNG){: .align-center}

Autoencoder의 구조는 다변량 통계적 방법 중 차원을 축소 시켜주는 PCA구조와 비슷하다.
Autoencoder에서 encoder와 decoder 과정에서는 주로 neural network가 사용되는데,
이 과정이 linear하다면, PCA방법을 사용한 것과 같은 결과를 가진다.
즉, Autoencoder는 PCA를 일반화한 방법이라고 할 수 있다.

다음으로는 Autoencoder의 종류를 설명할 것인데, 그 전에 Bias-Variance Tradeoff 관계에 대하여 먼저 설명하겠다.

## Bias-Variance Tradeoff

![](/assets/images/introduction of autoencoder/슬라이드6.PNG){: .align-center}

통계학에서 MSE는 Bias의 제곱값과 Variance의 합으로 나타낸다. 이 값이 작을수록 모델이 잘 적합된다고 할 수 있다.
그러나 Bias와 Variance는 서로 상충관계를 가진다.

Model을 underfitting하면, Bias는 증가하고, Variance는 작아진다.
반대로 Model을 overfitting하면, Variance가 증가하고, Bias는 감소한다.

Autoencoder는 재구성된 output이 input과 유사하기를 원하고, input 데이터를 의미있는 작은데이터로 압축시키고 싶어한다. 그러나 최적의 모델을 바로 찾는 것은 어려움이 있다. Autoencoder는 최적의 모형을 찾기 위해 모델 복잡도를 높힌 후, 다양한 규제 방법을 통해 먼저 Bias를 높혀 놓고, Variance를 낮추는 방향으로 발전했다.
앞서 말한 규제 기법으로 다양한 오토인코더의 종류가 탄생하였다.


# 3. Types of Autoencoders

## Sparse Autoencoder
![](/assets/images/introduction of autoencoder/슬라이드7.PNG){: .align-center}

첫 번째 규제 기법은 Sparse Autoencoder이다. Sparse Autoencoder는 hidden layer의 activation에 규제를 가하는 방법이다. 규제를 가하기 위해 Loss term에 L1-regulariztion 혹은 KL-divergence를 추가하여 사용한다.

## Denoising Autoencoder
![](/assets/images/introduction of autoencoder/슬라이드8.PNG){: .align-center}

두 번째 규제 기법은 Dinoising Autoencoder이다. Denoising Autoencoder는 input데이터에 random noise를 추가하여 손상된 데이터를 input 데이터로 사용하고, 여기서 나온 output을 기존 input데이터와 비교하여 학습을
진행한다. Denoising Autoencoder를 사용하면 작은 변화에 덜 민감하도록 모델을 생성할 수 있다.

## Contractive Autoencoder
![](/assets/images/introduction of autoencoder/슬라이드9.PNG){: .align-center}

세 번째 규제 기법은 Contractive Autoencoder이다.
Contractive Autoencoder 또한 Denoising Autoencoder와 유사하게,
작은 변화에 덜 민감하도록 특징을 추출하는 방법이다.
중요한 부분을 찾아내기 위해 Loss function에 Jacobian matrix를 추가하는데,
Jacobian matrix가 1차 도함수를 모아 놓은 행렬이기 때문에, 중요한 부분을
찾아 낼 수 있다.

예를 들어 숫자 3, 8을 학습시킬 때, 두 숫자의 차이는 왼쪽에서 차이가 나는데,
자코비안 행렬을 추가하여, 왼쪽이 중요한 부분임을 학습시킬 수 있다.

# 4. Applications of Autoencoders

이제 Autoencoder가 어떻게 활용되고 있는지 설명하겠다.

## a generative model
![](/assets/images/introduction of autoencoder/슬라이드10.PNG){: .align-center}

먼저 오토인코더는 모델을 생성하는데, 사용이 되고 있다.
이후 설명할 Variational Autoencoder와 GAN의 방법이 여기에 포함된다.


## classification

![](/assets/images/introduction of autoencoder/슬라이드11.PNG){: .align-center}

다음으로 오토인코더는 classification에 사용된다.
Autoencoder에서 encoder 부분을 특징 추출기로 사용하는데, 동일 라벨을 가진 샘플이 어떤 latent presentation에 해당 되어야한다는 가정을 통해 분류한다.

## clustering
![](/assets/images/introduction of autoencoder/슬라이드12.PNG){: .align-center}

Autoencoder는 군집을 분석하는데도 사용된다. 이는 classification방법과 유사한데,
Clustering을 진행할 때, 대부분은 차원의 저주에 시달린다. 따라서 Autoencoder를 통해 데이터를 압축하고, 압축한 데이터를 통해 군집을 나누는 방법입니다. 기존 loss 함수에 재구성 loss를 포함하여 사용합니다.


## anomaly detection
![](/assets/images/introduction of autoencoder/슬라이드13.PNG){: .align-center}

Autoencoder를 이용하여 이상치 탐지 또한 할 수 있다.
정상 관측치와 이상치의 latent representation이 다르다는 가정 하에,
정상 관측치로만 autoencoder 모델을 학습시킨다.
학습 후 test data와 test data를 넣어서 나온 output의 차이가 크다면 해당 데이터를
이상치로 판단합니다. 즉, 재구성 loss가 클 경우 이상치로 판단한다.

## recommendation systems
![](/assets/images/introduction of autoencoder/슬라이드14.PNG){: .align-center}

추천 시스템에도 Autoencoder가 사용된다.
CF 과정을 이용하여 추천을 하고, 인간의 선호도가 높은 상관관계를 가진다는 가정과
사람의 선호도는 과거와 미래가 유사하다는 가정을 통해 추천 시스템에 적용한다.

## dimensionality reduction
![](/assets/images/introduction of autoencoder/슬라이드15.PNG){: .align-center}

마지막으로 차원을 축소시킬 때, Autoencoder를 사용한다.
실제 데이터는 고차원, 희소한 벡터로 대부분 표현된다.
고차원에서 학습을 진행할시 차원의 저주에 걸릴 수 있기 때문에,
저차원으로 변환시켜 학습을 진행하는 것이 목적이다.
차원 축소의 대표적인 예시는 PCA 방법이 있다.


# 5. Variational Autoencoder(VAE)

## Variational Autoencoder란
![](/assets/images/introduction of autoencoder/슬라이드16.PNG){: .align-center}

이제 Variational Autoencoder에 대해 설명하도록 하겠다.
VAE는 다른 오토인코더들과 사용하는 목적이 다르다.
오토인코더가 정보를 잘 압축하기 위한 encoder부분에 집중을 했다하면,
VAE는 모델을 생성하기 위한 decoder부분에 집중을 하는 방법이다.
VAE는 latent variable을 통해 데이터를 생성하는 것이 목적인데, 데이터를 정확히 생성하기
위해서 encoder부분을 사용해야했기 때문에 오토인코더와 비슷한 구조를 가지게 되었다.

## Structural differences
![](/assets/images/introduction of autoencoder/슬라이드17.PNG){: .align-center}

VAE의 구조는 다음 그림과 같다.
Autoencoder에서 encoder를 거치면 바로 latent variable Z가 나오는 반면,
VAE에서는 encoder과정을 거치면 두 벡터 mu와 sigma가 나온다.
두 벡터를 정규분포로 만들고 분포에서 sampling을 통해 Z를 추출하는 것이 VAE의 방법이다.
샘플링을 할 때, Reparameterization Trick 방법을 사용하는데, N(0,1)을 따르는
\\[ε_i\\]를 추출하여 mu + sigma*e의 형태를 만들어서 샘플링 한다.
reparameterization Trick을 사용하는 이유와 방법은 후에 자세히 설명하도록 하겠다.

## Loss function
![](/assets/images/introduction of autoencoder/슬라이드18.PNG){: .align-center}

모델을 잘 학습시키기 위해, VAE의 Loss function은 두 부분의 합으로 이루어졌다.

첫 번째는 Reconstruction Error이다. Reconstruction Error는 앞에서 말했듯이 input과 output의 차이이다.

두 번째는 Regularization이다. 앞서 latent variable z가 정규분포를 따른다고 했었다.
encoder를 통과하는 확률이 정규분포를 따를 수 있도록 최적화해야한다.

지금부터 VAE가 이러한 Loss function을 가지게 된 과정을 유도해보도록 하겠다.

### Loss function 유도
![](/assets/images/introduction of autoencoder/슬라이드19.PNG){: .align-center}

VAE는 z를 통해 x를 잘 생성하고 싶은 것에서 시작한다.
z가 prior distribution p_theta(z)로 생성되고, x가 조건부분포 p_theta(x|z)
로 생성이 된다.

여기서 VAE는 x가 나올 확률이 높은 확률분포를 찾길 원하고,
p_theta(x)를 최대화 하는 theta 값을 찾으면 되는데, 계산이 불가능하다.
p_theta(x)를 x,z에 대한 결합분포로 바꾸고 이를 체인룰을 이용하여 바꾼 식 또한 계산이 불가능하다.

그래서 논문에서는 Variational inference방법을 사용하여
p_theta(z|x)의 근사치인 q_pi(z|x)를 이용하여 x의 maximum likelihood를 찾는다.

![](/assets/images/introduction of autoencoder/슬라이드20.PNG){: .align-center}

q_pi(z|x)를 이용한 방법은 다음과 같다.
먼저 log를 씌워도 p_theta(x)를 최대화하는 방법은 같기 떄문에 log를 씌우겠다.
z가 q_pi(z|x)를 따르기 떄문에,
위처럼 q_pi(z|x)를 곱한 z에 대한 적분식으로 나타낼 수 있다.

베이즈 정리를 이용하여 다음과 같은 식으로 쓸 수 있고,
log 안에 분모 분자에 q_pi(z|x)를 곱해준 후, 로그의 성질을 이용하여
덧셈과 로그를 분해해 준다.
이 식에서 앞 두 항을 L로 표현하고, KL Divergence의 성질을 이용하면 식을 다음과 같이 표현할 수 있다.


![](/assets/images/introduction of autoencoder/슬라이드21.PNG){: .align-center}

정리하면 log(p_theta(x))가 L과 KL divergence로 표현되고,
KL divergence는 항상 0보다 크거나 같기 때문에
L이 log(p_theta(x))의 증거 하한 ELBO가 된다.

여기서 MLE를 최대화 시키려면, ELBO를 maximaze하면 된다.


![](/assets/images/introduction of autoencoder/슬라이드22.PNG){: .align-center}

ELBO를 최대화하기 위해, L에 대한 식을 다시 가져오겠다.
여기서 로그를 뻴셈으로 분해해주고
p_theta(x|z)p_theta(z)를 z,x의 결합분포로 나타내준다.
z가 q_pi(z|x)를 따르기 떄문에
기댓값으로 바꿔줄 수 있다.

위 식은 q_pi(z|x)로 샘플링 후 샘플링 값을 이용해 monte-carlo방법을 통해 계산이 가능하다.

monte-carlo방법을 사용하면 variance가 있는데, 그러나 이 방법은 Variance가 크게 나타난다.

![](/assets/images/introduction of autoencoder/슬라이드23.PNG){: .align-center}

Variance를 줄이기 위해, L에 대한 식을 다른 방법으로 바꾸어 보겠다.

2번째 줄까지는 앞에서 설명한 것과 같고,
log 분해를 다음과 같은 방식으로 해준다.
이렇게하면 L을 기댓값과 KL divergence로 나타낼 수 있는데,
KL divergence 부분은 variance가 없기 때문에, 앞에서 설명한 식보다 variance가 줄어들게 된다.

![](/assets/images/introduction of autoencoder/슬라이드24.PNG){: .align-center}

앞에서 VAE의 loss function의 Loss function은 두 부분의 합으로 이루어졌다고 했는데,
방금 전 식을 보면 기대값 부분이 reconstruction error를 나타내고, KL div가 regularization을 나타낸다.
논문에 Regularization부분을 계산하는 방법이 나와있는데, 두가지 가정이 필요하다.
첫번째 가정은 q가 정규분포를 따른다이고, 두번째는 p는 표준 정규분포를 따른다고 가정한다.

![](/assets/images/introduction of autoencoder/슬라이드25.PNG){: .align-center}

정규분포를 따르는 두 분포의 KL divergence는 다음과 같이 계산되어서,
위 식에 값을 대입해주면 다음과 같은 식이 나오게 된다.
위 식에 encoder를 통해 나온 값을 대입하면 KL divergence값을 구할 수 있다.

![](/assets/images/introduction of autoencoder/슬라이드26.PNG){: .align-center}

이제 reparameterization Trick에 대해 설명하겠다.
앞서 VAE 과정에서 encoder 부분에 reparameterzation Trick 과정이 존재했다.

![](/assets/images/introduction of autoencoder/슬라이드27.PNG){: .align-center}

이 과정이 존재하는 이유는 딥러닝을 학습할 때는 역전파 과정을 수행하는데,
역전파를 수행하기 위해서는 미분이 필요하다.
그러나 encoder과정 중 sampling을 바로 N(mu,sigma)에서 뽑으면 미분이 불가능하다.

따라서 표준정규분포를 따르는 e를 이용하여 z를 mu + sigma*e로 나타낸다.
이렇게 변환을 시켜주면 미분이 가능하기 때문에 역전파를 수행할 수 있다.

![](/assets/images/introduction of autoencoder/슬라이드28.PNG){: .align-center}

마지막으로 VAE 과정을 정리하면, encoder과정을 통해 mu와 sigma가 나오고 이를 정규분포화하고 sampling해서
z를 생성한다. 생성된 z를 decoder에 통과시켜 data를 생성하는것이 VAE 모델이다.
