---
layout: single
title: "깃허브 블로그 활용법"
categories: "blog"
tag: [blog]
toc: true
---

해당 내용은 깃허브 블로그 작성법을 공부하면서 작성한 게시글입니다.



# 1. 이미지 삽입 방법
먼저 이미지를 지정 경로에 저장한다.

/assets/images/ 위치에 저장해보겠다.

## 1.1 마크다운(.md)에서 삽입 방법
마크다운에서 이미지 삽입 방법은 다음과 같다.
```python
# ![Alt text](/파일경로/파일이름)
![ball](/assets/images/ball_images.jpeg)
```
[Alt text]에는 이미지를 알아볼 수 있게 이름을 넣어준다.

![ball](/assets/images/ball_images.jpeg)

[이미지 예시]

다음과 같이 이미지가 추가된 것을 볼 수 있다.


## 1.2 html에서 삽입 방법
html에서는 다음과 같은 방법으로 추가할 수 있다.
```python
# <img src= "파일 경로 및 이름">
<img src= "/assets/images/ball_images.jpeg"> 
```
<img src= "/assets/images/ball_images.jpeg">

[이미지 예시]

마찬가지로 이미지가 추가된 것을 확인할 수 있다.