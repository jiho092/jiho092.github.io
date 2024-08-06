---
layout: single
title: "깃허브 블로그 활용법"
excerpt : ""
categories: 
    - Blog #카테고리설정
tag: 
    - ["Blog"] #테그
# toc: true
# author_profile: false
# toc_sticky: true
# toc_label: 목차
# sidebar:
#     nav: "docs"
---

이 글은 깃허브 블로그 작성법을 공부하면서 작성한 게시글입니다.

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

## 1.3 이미지 위치 설정방법
align을 사용하면 이미지의 위치를 설정할 수 있다.
```python
{: .align-center} #가운데 정렬
{: .align-left} #왼쪽 정렬
{: .align-right} #오른쪽 정렬

![ball](/assets/images/ball_images.jpeg){: .align-center}
```
![ball](/assets/images/ball_images.jpeg){: .align-center}



# 2. 공지 띄우기
쉽게 알아보기 위해 다음과 같은 코드를 작성한다.
공지사항 아래에 작성하면 나타난다.


## 2.1 한줄 띄우기
```python
{: .notice--danger} #빨강
{: .notice--primary} #회색
{: .notice--warning} #주황
{: .notice--info} #하늘
{: .notice--success} #초록
```

빨간색
{: .notice--danger}
회색
{: .notice--primary}
주황색
{: .notice--warning}
하늘
{: .notice--info}


## 2.2 여러줄 띄우기
```python
<div class='notice--success'>
<h4> 문장을 넣을 수도 있습니다. </h4>
<ui>
    <li>공지사항 순서1</li>
    <li>공지사항 순서2</li>
    <li>공지사항 순서3</li>
</ui>
</div>
```

<div class='notice--success'>
<h4> 여러줄 넣기. </h4>
<ui>
    <li>공지사항 순서1</li>
    <li>공지사항 순서2</li>
    <li>공지사항 순서3</li>
</ui>
</div>


# 3. 버튼 추가하기
입력한 주소로 이동하는 버튼을 추가할 수 있다.
```python
[버튼](https://google.com){: .btn .btn--danger}
```
[버튼](https://google.com){: .btn .btn--danger}


# 4. MathJax로 수학식 표현하기

[블로그 링크](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/){: .btn .btn--primary}



해당 블로그의 방법을 참고하였다.


$f(x) = x^2+3x+2$

# 5. text centering

```python
<p align="center"> 
```
<p align="center"> 이 코드를 사용하고 text를 입력하면 글을 가운데로 정렬할 수 있다.

# 6. VS code viewer(MarkDown preview)

Ctrl + Shift + V