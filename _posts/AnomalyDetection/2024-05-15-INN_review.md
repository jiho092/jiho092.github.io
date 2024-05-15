---
layout: single
title: "INN method paper review" #제목
excerpt : ""
categories: 
    - anomaly_detection #카테고리설정
tag: 
    - ["image","anomaly", "ood","noisy label"] #테그

date: 2024-05-15
last_modified_at: 2024-05-15
classes: wide    
---

# 0. Abstract

noise label 문제를 다룰 때, 좋은 성능을 보였던 모델들은 memorization effect를 loss로 사용하는 전략을 사용했다.
그러나 memorization effect 방법은 train epoch에 민감하고, 데이터가 심하게 오염되거나 imbalance한 경우에는 좋은 성능을 보이지 못했다.

그래서 이 논문에서는 noise가 섞인 데이터로 부터 clean label을 재정의하는 새로운 기법인 INN(Integration with the Nearest Neighborhoods) 기법을 제시한다.
이 방법은 clean label의 neighbor region의 패턴이 training epoch에 상관없이 noisy label data와 일관되게 다르다는 것으로 부터 시작한다.

INN method는 계산량이 많지만 안정적이고 좋은 성능을 보인다.

# 1. Introduction

