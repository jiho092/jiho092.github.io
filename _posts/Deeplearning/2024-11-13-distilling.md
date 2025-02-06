---
layout: single
title: "Distilling Semantic Priors from SAM to Efficient Image Restoration Models" #제목
excerpt : ""
categories: 
    - deeplearningCV #카테고리설정
tag: 
    - [""] #테그

use_math: true

date: 2024-11-13
last_modified_at: 2024-11-13
#classes: wide    
---
[Distilling Semantic Priors from SAM to Efficient Image Restoration Models](https://arxiv.org/pdf/2403.16368){: .btn .btn--primary}

## Abstract

&nbsp;&nbsp;이미지 복원(IR)에서 세분화 모델의 의미 프라이어(semantic priors)를 활용하는 것은 성능을 향상시키기 위한 일반적인 접근 방식이다. 최근 등장한 Segment Anything Model(SAM)은 IR 작업을 강화하기 위해 고급 의미 프라이어를 추출하는 강력한 도구로 자리잡았다. 그러나 SAM의 높은 계산 비용은 기존의 더 작은 IR 모델에 비해 IR 작업에서 큰 부담이 된다. SAM을 활용한 의미 프라이어 추출은 모델의 추론 효율성을 상당히 저해한다. 

&nbsp;&nbsp;이 문제를 해결하기 위해, 논문에서는 SAM의 의미 지식을 IR 모델의 성능 향상을 위해 추론 과정에 간섭하지 않으면서 적용할 수 있는 일반적인 프레임워크를 제안한다. 논문의 프레임워크는 **Semantic Priors Fusion (SPF) 기법**과 **Semantic Priors Distillation (SPD) 기법**으로 구성됩니다. SPF는 원래 IR 모델이 예측한 복원된 이미지와 SAM이 예측한 의미 마스크 간의 두 가지 정보를 융합하여 정제된 복원 이미지를 생성한다. SPD는 자기 증류(self-distillation) 방식을 사용하여 융합된 의미 프라이어를 원래 IR 모델의 성능 향상을 위해 전달한다. 

&nbsp;&nbsp;또한, SPD를 위해 **Semantic-Guided Relation (SGR) 모듈**을 설계하여 의미적 특징 표현 공간의 일관성을 확보하고, 프라이어를 완전히 증류할 수 있도록 한다. 우리는 제안된 프레임워크가 강우 제거, 블러 제거, 노이즈 제거를 포함한 다양한 IR 모델 및 작업에서 효과적임을 입증한다.
