---
layout: single
title: "MentorNet paper review" #제목
excerpt : ""
categories: 
    - anomalydetection #카테고리설정
tag: 
    - ["classification","anomaly", "ood","noisy label"] #테그

date: 2024-05-15
last_modified_at: 2024-05-15
#classes: wide    
---

# 0. Abstract
supervised learning에서 학습을 진행할 때에는 데이터가 정확한 Label을 가진다고 가정한다. 그러나 현실에서 얻어지는 데이터에는 Nosie가 포함될 수 있다. 이러한 경우, 모델의 수렴속도가 오래 걸리지만 결국은 모델이 수렴하게 된다. 즉, 잘못된 데이터도 모두 학습을 시키는 것이기 때문에, 모델의 성능이 떨어질 수 밖에 없다. 이 논문에서는 이러한 문제를 극복하기위해 MentorNet을 제안한다. 