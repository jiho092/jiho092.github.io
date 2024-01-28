---
#layout: single
title: "에어비앤비 크롤링" #제목
excerpt : "에어비앤비에서 데이터를 크롤링합니다."
categories: 
    - Project #카테고리설정
tag: 
    - ["파이썬"] #테그
---



# 1. 에어비앤비 검색 패턴 파악하기

## 1.1크롤링을 위해 두 모듈 사용

```python
import requests
from scrapy.selector import Selector
```

## 1.2 에어비앤비에서 URL주소가 메인 홈페이지 이후 도시 이름과 필터조건(주말, 2인 기준)으로 검색하는 URL 링크가 다음과 같음 
###  숙소 검색이 지도 기반중심으로 검색이 된다.


```python
city = '성산읍'
main_url = 'https://www.airbnb.co.kr'
city_url = f'{main_url}/s/{city}/homes?tab_id=home_tab&refinement_paths%5B%5D=%2Fhomes&price_filter_input_type=0&price_filter_num_nights=5&date_picker_type=flexible_dates&flexible_trip_lengths%5B%5D=weekend_trip&adults=2&source=structured_search_input_header&search_type=filter_change'
#성산읍 주말 2인 기준으로 검색

# 셀렉터 만들기
html = requests.get(city_url).content
sel  = Selector(text=html)

# 숙박정보 가져오기
hotels = sel.css('div.c4mnd7m')
print('Number of hotels:', len(hotels)) #20개 출력이 맞는지 확인

# 다음페이지 버튼 찾기
next_page = sel.css('a._1bfat5l ::attr(href)').extract_first()
print('Next page:', '\n', next_page) #링크가 맞는지 확인
```

    Number of hotels: 20
    Next page: 
     /s/성산읍/homes?tab_id=home_tab&refinement_paths%5B%5D=%2Fhomes&price_filter_input_type=0&price_filter_num_nights=2&date_picker_type=flexible_dates&flexible_trip_lengths%5B%5D=weekend_trip&adults=2&source=structured_search_input_header&search_type=filter_change&query=%EC%84%B1%EC%82%B0%EC%9D%8D%2C%20%EC%84%9C%EA%B7%80%ED%8F%AC%EC%8B%9C%2C%20%EC%A0%9C%EC%A3%BC%ED%8A%B9%EB%B3%84%EC%9E%90%EC%B9%98%EB%8F%84%2C%20%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD&place_id=ChIJ72mL20ISDTURZADSM6lufuY&federated_search_session_id=80299783-641b-42a8-a785-4418465cebb6&pagination_search=true&cursor=eyJzZWN0aW9uX29mZnNldCI6MiwiaXRlbXNfb2Zmc2V0IjoyMCwidmVyc2lvbiI6MX0%3D
    

## 1.3 검색 element 파악


```python
hotel = hotels[0] #첫번째 숙소에서 대표적으로 크롤링 실시
```


```python
hotel.css('div.t1jojoys ::text').extract_first() #name 정보 추출
```




    '성산읍, 서귀포시의 집'




```python
hotel.css('div._1jo4hgw ::text').extract_first() #1박당 가격 추출
```




    '₩182,589'




```python
hotel.css('span.r1dxllyb ::text').extract_first() #별점 및 리뷰수 추출
```




    '4.99 (120)'




```python
hotel.css('span.t6mzqp7 ::text').extract_first() #detail 정보 추출
```




    '제주감성숙소 /넷플릭스/빔프로젝터/하트욕조//프라이빗정원/2인숙소\n(취사×바베큐×)'




```python
hotel.css('div._tt122m ::text').extract_first() #총 가격 추출
```




    '총액 ₩365,177'



# 2. 데이터 수집


```python
cities = ['성산읍','표선면','남원읍','서귀포','중문동','안덕면','대정읍','한경면','한림읍','애월읍','제주시','조천읍','구좌읍']
#제주 행정구역을 모두 저장

#5개의 변수를 저장할 리스트 만들기
zoom = '13' #배율이 13배가 되도록 설정
name = []
detail_info = []
per_price = []
total_price = []
star_info = []
    
    

for k in range(len(cities)): #행정구역을 돌림
    
    main_url = 'https://www.airbnb.co.kr'
    city_url = f'{main_url}/s/{cities[k]}/homes?tab_id=home_tab&refinement_paths%5B%5D=%2Fhomes&price_filter_input_type=0&price_filter_num_nights=5&date_picker_type=flexible_dates&flexible_trip_lengths%5B%5D=weekend_trip&adults=2&zoom={zoom}&source=structured_search_input_header&search_type=filter_change'
    html = requests.get(city_url).content
    sel  = Selector(text=html)
    hotels = sel.css('div.c4mnd7m')
    for i in range(14): #15페이지 까지이므로 14번 돌려야함 1페이지 -> 15페이지
        for j in range(20): #한페이지에는 20개의 숙소 정보가 나옴
            name.append(hotels[j].css('div.t1jojoys ::text').extract_first()) #name 저장
            detail_info.append(hotels[j].css('span.t6mzqp7 ::text').extract_first()) #detail_info 저장
            per_price.append(hotels[j].css('div._1jo4hgw ::text').extract_first()) #per_price 저장
            total_price.append(hotels[j].css('div._tt122m ::text').extract_first()) #total_price 저장
            star_info.append(hotels[j].css('span.r1dxllyb ::text').extract_first()) #star_info 저장
        
        next_page = sel.css('a._1bfat5l ::attr(href)').extract_first()
        city_url = f'{main_url}{next_page}'
        html = requests.get(city_url).content
        sel  = Selector(text=html)
        hotels = sel.css('div.c4mnd7m')
    
    #마지막 15페이지 크롤링
    for j in range(20):
        name.append(hotels[j].css('div.t1jojoys ::text').extract_first())
        detail_info.append(hotels[j].css('span.t6mzqp7 ::text').extract_first())
        per_price.append(hotels[j].css('div._1jo4hgw ::text').extract_first())
        total_price.append(hotels[j].css('div._tt122m ::text').extract_first())
        star_info.append(hotels[j].css('span.r1dxllyb ::text').extract_first())

```

# 3. 수집한 데이터를 데이터프레임으로 저장


```python
import pandas as pd
AIRBNB = pd.DataFrame()
AIRBNB['name'] = name
AIRBNB['detail_info'] = detail_info
AIRBNB['per_price'] = per_price
AIRBNB['total_price'] = total_price
AIRBNB['star_info'] = star_info

AIRBNB
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>detail_info</th>
      <th>per_price</th>
      <th>total_price</th>
      <th>star_info</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>성산읍, 서귀포시의 집</td>
      <td>제주감성숙소 /넷플릭스/빔프로젝터/하트욕조//프라이빗정원/2인숙소\n(취사×바베큐×)</td>
      <td>₩182,589</td>
      <td>총액 ₩365,177</td>
      <td>4.99 (120)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>성산읍, 서귀포시의 게스트용 별채</td>
      <td>"1259 studio" 제주도 서귀포 감성 돌집 자쿠지 민박</td>
      <td>₩222,530</td>
      <td>총액 ₩445,059</td>
      <td>4.75 (172)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>성산읍, 서귀포시의 아파트</td>
      <td>소문난  Oceanstar레저콘도</td>
      <td>₩90,153</td>
      <td>총액 ₩180,306</td>
      <td>NEW</td>
    </tr>
    <tr>
      <th>3</th>
      <td>성산읍, 서귀포시의 펜션</td>
      <td>제주 동부 성산일출봉 감성숙소 세오니네민박</td>
      <td>₩85,589</td>
      <td>총액 ₩171,177</td>
      <td>4.67 (209)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>성산읍, 서귀포시의 개인실</td>
      <td>#올리브# 제주에서 성산일출봉 제일 가까운 숙소#연박시10%할인#넷플릭스제공</td>
      <td>₩86,220</td>
      <td>총액 ₩157,140</td>
      <td>4.94 (71)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3895</th>
      <td>구좌읍의 펜션</td>
      <td>제주돌채 [안돌채]: 햇살 가득한 툇마루가 있는 공간 Jeju Private Stay</td>
      <td>₩262,471</td>
      <td>총액 ₩524,941</td>
      <td>4.93 (276)</td>
    </tr>
    <tr>
      <th>3896</th>
      <td>구좌읍, 제주시의 전원주택</td>
      <td>호루호루/한적하고 아늑한 동쪽 바닷가 마을 2층 전원주택 /편안한 단독주택</td>
      <td>₩273,883</td>
      <td>총액 ₩547,765</td>
      <td>4.96 (102)</td>
    </tr>
    <tr>
      <th>3897</th>
      <td>구좌읍, 제주시의 전원주택</td>
      <td>제주 동쪽 평대 해변가 아서의 집</td>
      <td>₩205,412</td>
      <td>총액 ₩410,824</td>
      <td>4.96 (48)</td>
    </tr>
    <tr>
      <th>3898</th>
      <td>구좌읍, 제주시의 개인실</td>
      <td>평대리[제주나봄]독채펜션-봄동 프라이빗 노천탕</td>
      <td>₩251,059</td>
      <td>총액 ₩502,118</td>
      <td>5.0 (29)</td>
    </tr>
    <tr>
      <th>3899</th>
      <td>성산읍, 서귀포시의 펜션</td>
      <td>성산일출봉뷰 투룸 감성가득 영화관 (침대방,넷플릭스방)</td>
      <td>₩119,824</td>
      <td>총액 ₩239,647</td>
      <td>4.72 (43)</td>
    </tr>
  </tbody>
</table>
<p>3900 rows × 5 columns</p>
</div>




```python
AIRBNB.to_csv('C:/Users/leeji/Desktop/AIRBNB.csv', index = False)
```
