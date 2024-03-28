---
layout: single
title: "이미지 이상치 탐지" #제목
excerpt : ""
categories: 
    - anomaly_detection #카테고리설정
tag: 
    - ["image","anomaly", "ood"] #테그

date: 2024-03-28
last_modified_at: 2024-03-28
classes: wide    
---


방법론
 - student-Teacher method : Teacher output의 regression을 통해 분포를 학습
 - Non-parametric Inference : embedding들의 memory bank로 분포 형성
 - Reconstruction : 재구축을 통해 분포 학습
 - Generative : 데이터들의 분포를 직접 학습

결과
 - Image-level
 - pixel-level

대부분 정상 데이터만을 통해 학습을 진행 

PaDiM: a Patch Distribution Modeling Framework for Anomaly Detection and Localization

PatchCore: Towards Total Recall in Industrial Anomaly Detection

Ganomaly: Semi-Supervised Anomaly Detection via Adversarial Training

CFLOW-AD: Real-Time Unsupervised Anomaly Detection with Localization via Conditional Normalizing Flows