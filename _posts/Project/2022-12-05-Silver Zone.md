---
#layout: single
title: "노인 사고 예방을 위한 분석"

categories: 
    - Project #카테고리설정
tag: 
    - ["파이썬"] #테그
---






```python
import pandas as pd
```

# 데이터 전처리

# 1. 사고 건수


```python
acc = pd.read_csv("accidentInfoList.csv")

#해당 파일에서 행정구역에 포함된 사고 건수를 수집
```


```python
city = ['종로구','중구','용산구','성동구','광진구','동대문구','중랑구','성북구','강북구','도봉구','노원구','은평구','서대문구','마포구','양천구','강서구','구로구','금천구','영등포구','동작구','관악구','서초구','강남구','송파구','강동구']
city[1]
acc_num = []
for i in range(len(city)):
    acc_num.append(acc[acc['시군구'].str.contains(city[i])].shape[0])
```

# 2. 노인보호구역


```python
protect = pd.read_csv("노인보호구역.csv")
protect.head()

#해당 파일에서 행정구역에 포함된 노인보호구역수를 수집
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
      <th>연번</th>
      <th>자치구</th>
      <th>시설명</th>
      <th>위치</th>
      <th>지정일</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>종로구</td>
      <td>종로노인종합복지관</td>
      <td>율곡로19길 17-8</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>종로구</td>
      <td>종로노인종합복지관 무악센터</td>
      <td>통일로14길 30</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>종로구</td>
      <td>서울노인복지센터</td>
      <td>삼일대로 467</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>종로구</td>
      <td>락희거리</td>
      <td>삼일대로 428</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>종로구</td>
      <td>종묘광장공원 동순라길</td>
      <td>인의동</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
</div>




```python
protect_num = []
for i in range(len(city)):
    protect_num.append(protect[protect['자치구'].str.contains(city[i])].shape[0])
```

# 3.횡단보도


```python
cross = pd.read_csv("서울시 대로변 횡단보도 위치정보.csv")


#해당 파일에서 행정구역에 포함된 횡단보도수를 수집
```


```python
cross_num = []
for i in range(len(city)):
    cross_num.append(cross[cross['시군구명'].str.contains(city[i])].shape[0])
```

# 4. 보행작동 신호기


```python
!pip install geopy
```



```python
!pip install geocoder
```






```python
import pandas as pd
import time
import geocoder
from geopy.geocoders import Nominatim
```


```python
df2 = pd.read_csv("보행자_작동신호기.csv")
```


```python
geolocator = Nominatim(user_agent="user",timeout=10)
location = geolocator.geocode("Dnipro, Dnipropetrovsk, UA")
(location.latitude, location.longitude)

#위도경도값을 입력하면 해당 주소를 추출해주는 geopy 모듈 사용
loca = geolocator.reverse("37.5409, 126.8638")
loca.address
```




    '목3동우체국, 등촌로, 목3동, 양천구, 서울, 07786, 대한민국'




```python
add2 = []
```


```python
#주소 추출
for i in range(12377):
    location2 =geolocator.reverse('{} , {}'.format(df2['위도'][i],df2['경도'][i]))
    add2.append(location2.address)
    if i % 20 == 0:
        print(i,'times')
```


```python
df2['address'] = add2
df2
```


```python
#업데이트한 파일 저장
df.to_csv('보행_작동신호기.csv', index = False)
```


```python
bohang = pd.read_csv("보행_작동신호기.csv")

#해당 파일에서 행정구역에 포함된 보행작동 신호기 수를 수집
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
      <th>index</th>
      <th>위도</th>
      <th>경도</th>
      <th>address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>37.539064</td>
      <td>126.970764</td>
      <td>101동, 원효로90길, 원효로1동, 용산구, 서울, 04368, 대한민국</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>37.459697</td>
      <td>127.054412</td>
      <td>탑성마을, 헌릉로, 내곡동, 서초구, 서울, 06794, 대한민국</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>37.534891</td>
      <td>127.034490</td>
      <td>성수대교, 고산자로, 성수1가1동, 성동구, 서울, 04768, 대한민국</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>37.526613</td>
      <td>126.870158</td>
      <td>목1동, 양천구, 서울, 08000, 대한민국</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>37.580993</td>
      <td>126.881956</td>
      <td>709동, 상암산로1길, 12통, 상암동, 마포구, 서울, 03905, 대한민국</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>875</th>
      <td>875</td>
      <td>37.666432</td>
      <td>127.034273</td>
      <td>원약국, 도당로13다길, 방학2동, 도봉구, 서울, 01382, 대한민국</td>
    </tr>
    <tr>
      <th>876</th>
      <td>876</td>
      <td>37.548273</td>
      <td>126.984452</td>
      <td>후암약수터, 소월로, 경리단길, Huam-dong, 용산구, 서울, 04339, 대한민국</td>
    </tr>
    <tr>
      <th>877</th>
      <td>877</td>
      <td>37.531349</td>
      <td>126.877360</td>
      <td>신월여의지하도로, 목1동, 양천구, 서울, 07992, 대한민국</td>
    </tr>
    <tr>
      <th>878</th>
      <td>878</td>
      <td>37.531283</td>
      <td>126.877456</td>
      <td>목동종합운동장, 939, 안양천로, 목1동, 양천구, 서울, 07985, 대한민국</td>
    </tr>
    <tr>
      <th>879</th>
      <td>879</td>
      <td>37.666490</td>
      <td>127.034425</td>
      <td>원약국, 도당로13길, 방학동, 도봉구, 서울, 01391, 대한민국</td>
    </tr>
  </tbody>
</table>
<p>880 rows × 4 columns</p>
</div>




```python
#지역별로 수집
bohang_num = []
for i in range(len(city)):
    bohang_num.append(bohang[bohang['address'].str.contains(city[i])].shape[0])
```

# 5. 잔여시간 표시기


```python
df = pd.read_csv("잔여시간_표시기.csv")
# 위경도 좌표만 나와있음
```


```python
#보행작동 신호기와 같은 방법으로 시행
add = []

for i in range(12377):
    add.append((geolocator.reverse('{} , {}'.format(df['위도'][i],df['경도'][i]))).address)
    if i % 20 == 0:
        print(i,'times')
```




```python
add.append(0)
#중간에 위도 경도 결측치가 있어서 0으로 채운후 다시 진행
for i in range(11933, 12377):
    add.append((geolocator.reverse('{} , {}'.format(df['위도'][i],df['경도'][i]))).address)
    if i % 20 == 0:
        print(i,'times')
        
df['address'] = add
df
```



```python
#새로운 파일 저장
df.to_csv('C:/Users/leeji/Desktop/잔여시간표시기_행정구역.csv', index = False)
```


```python
re_time = pd.read_csv("잔여시간표시기_행정구역.csv")

#해당 파일에서 행정구역에 포함된 사고 건수를 수집
```


```python
re_time.head()
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
      <th>Unnamed: 0</th>
      <th>위도</th>
      <th>경도</th>
      <th>address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>37.533469</td>
      <td>126.894055</td>
      <td>선유중학교, 선유로43가길, 양평2동, 영등포구, 서울, 07209, 대한민국</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>37.540925</td>
      <td>126.863890</td>
      <td>목3동우체국, 등촌로, 목3동, 양천구, 서울, 07786, 대한민국</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>37.562890</td>
      <td>126.851465</td>
      <td>양천로, 가양동, 강서구, 서울, 07574, 대한민국</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>37.537073</td>
      <td>126.837237</td>
      <td>가로공원로, 화곡1동, 강서구, 서울, 07726, 대한민국</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>37.495592</td>
      <td>126.840757</td>
      <td>고척로4길, 오류1동, 구로구, 서울, 08270, 대한민국</td>
    </tr>
  </tbody>
</table>
</div>




```python
#지역별로 나누어 저장
re_time_num = []
for i in range(len(city)):
    re_time_num.append(re_time[re_time['address'].str.contains(city[i])].shape[0])
```

# 6. 노인 인구수


```python
#노인인구수 파일 불러오기
sei = pd.read_csv("고령자 현황.csv")
sei.head()
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
      <th>시군구</th>
      <th>동</th>
      <th>총인원</th>
      <th>남자</th>
      <th>여자</th>
      <th>노인</th>
      <th>노인남</th>
      <th>노인여</th>
      <th>내국인</th>
      <th>내국남</th>
      <th>내국여</th>
      <th>외국인</th>
      <th>외국남</th>
      <th>외국여</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>종로구</td>
      <td>사직동</td>
      <td>9636</td>
      <td>4353</td>
      <td>5283</td>
      <td>1774</td>
      <td>779</td>
      <td>995</td>
      <td>1754</td>
      <td>767</td>
      <td>987</td>
      <td>20</td>
      <td>12</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종로구</td>
      <td>삼청동</td>
      <td>2739</td>
      <td>1302</td>
      <td>1437</td>
      <td>625</td>
      <td>273</td>
      <td>352</td>
      <td>601</td>
      <td>256</td>
      <td>345</td>
      <td>24</td>
      <td>17</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>종로구</td>
      <td>부암동</td>
      <td>9782</td>
      <td>4686</td>
      <td>5096</td>
      <td>1802</td>
      <td>800</td>
      <td>1002</td>
      <td>1794</td>
      <td>796</td>
      <td>998</td>
      <td>8</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>종로구</td>
      <td>평창동</td>
      <td>18329</td>
      <td>8542</td>
      <td>9787</td>
      <td>3435</td>
      <td>1499</td>
      <td>1936</td>
      <td>3418</td>
      <td>1488</td>
      <td>1930</td>
      <td>17</td>
      <td>11</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>종로구</td>
      <td>무악동</td>
      <td>8297</td>
      <td>3856</td>
      <td>4441</td>
      <td>1424</td>
      <td>610</td>
      <td>814</td>
      <td>1424</td>
      <td>610</td>
      <td>814</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
    </tr>
  </tbody>
</table>
</div>




```python
#구별 노인 인구수를 분리
sei_num=[]
for i in range(len(city)):
      globals()['df'+str(i)] = pd.DataFrame()
      globals()['df'+str(i)] = sei[sei['시군구'].str.contains(city[i])]
      globals()['num'+str(i)] = globals()['df'+str(i)]['노인'].sum()
      sei_num.append(globals()['num'+str(i)])  
```

# 7. 사고다발지역
 - 엑셀로 처리

# 최종 데이터


```python
dat = pd.DataFrame()
dat['행정구'] = city
dat['사고건수'] = acc_num
dat['노인보호구역수'] = protect_num
dat['횡단보도수'] = cross_num
dat['잔여시간표시기'] =re_time_num
dat['보행작동신호기'] = bohang_num
dat['노인인구수'] = sei_num
dat['사고다발구역'] = [1,3,1,0,0,7,1,5,5,3,3,5,5,1,3,4,2,0,2,4,4,0,0,1,3]
dat['다발구역건수'] = [5,16,5,0,0,46,5,26,25,15,15,25,25,5,15,20,10,0,10,24,20,0,0,5,15]
dat['다발구역사상자수'] = [5,16,5,0,0,50,5,27,27,15,18,26,28,5,16,22,10,0,12,28,20,0,0,5,17]

dat
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
      <th>행정구</th>
      <th>사고건수</th>
      <th>노인보호구역수</th>
      <th>횡단보도수</th>
      <th>잔여시간표시기</th>
      <th>보행작동신호기</th>
      <th>노인인구수</th>
      <th>사고다발구역</th>
      <th>다발구역건수</th>
      <th>다발구역사상자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>종로구</td>
      <td>43</td>
      <td>5</td>
      <td>1080</td>
      <td>313</td>
      <td>30</td>
      <td>27818</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>중구</td>
      <td>43</td>
      <td>8</td>
      <td>1098</td>
      <td>338</td>
      <td>29</td>
      <td>24392</td>
      <td>3</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>용산구</td>
      <td>32</td>
      <td>5</td>
      <td>917</td>
      <td>296</td>
      <td>42</td>
      <td>39070</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>성동구</td>
      <td>54</td>
      <td>5</td>
      <td>825</td>
      <td>386</td>
      <td>31</td>
      <td>46380</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>광진구</td>
      <td>40</td>
      <td>5</td>
      <td>1011</td>
      <td>401</td>
      <td>22</td>
      <td>51723</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>동대문구</td>
      <td>109</td>
      <td>0</td>
      <td>1318</td>
      <td>460</td>
      <td>46</td>
      <td>62211</td>
      <td>7</td>
      <td>46</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6</th>
      <td>중랑구</td>
      <td>67</td>
      <td>3</td>
      <td>1315</td>
      <td>401</td>
      <td>36</td>
      <td>71682</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>성북구</td>
      <td>75</td>
      <td>8</td>
      <td>1131</td>
      <td>474</td>
      <td>42</td>
      <td>74709</td>
      <td>5</td>
      <td>26</td>
      <td>27</td>
    </tr>
    <tr>
      <th>8</th>
      <td>강북구</td>
      <td>76</td>
      <td>2</td>
      <td>803</td>
      <td>366</td>
      <td>94</td>
      <td>64333</td>
      <td>5</td>
      <td>25</td>
      <td>27</td>
    </tr>
    <tr>
      <th>9</th>
      <td>도봉구</td>
      <td>61</td>
      <td>2</td>
      <td>767</td>
      <td>348</td>
      <td>74</td>
      <td>64160</td>
      <td>3</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <th>10</th>
      <td>노원구</td>
      <td>79</td>
      <td>6</td>
      <td>1165</td>
      <td>632</td>
      <td>38</td>
      <td>88345</td>
      <td>3</td>
      <td>15</td>
      <td>18</td>
    </tr>
    <tr>
      <th>11</th>
      <td>은평구</td>
      <td>73</td>
      <td>10</td>
      <td>1154</td>
      <td>424</td>
      <td>18</td>
      <td>87241</td>
      <td>5</td>
      <td>25</td>
      <td>26</td>
    </tr>
    <tr>
      <th>12</th>
      <td>서대문구</td>
      <td>66</td>
      <td>6</td>
      <td>723</td>
      <td>348</td>
      <td>30</td>
      <td>54268</td>
      <td>5</td>
      <td>25</td>
      <td>28</td>
    </tr>
    <tr>
      <th>13</th>
      <td>마포구</td>
      <td>39</td>
      <td>6</td>
      <td>1670</td>
      <td>481</td>
      <td>42</td>
      <td>54582</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>14</th>
      <td>양천구</td>
      <td>71</td>
      <td>7</td>
      <td>1652</td>
      <td>652</td>
      <td>51</td>
      <td>68627</td>
      <td>3</td>
      <td>15</td>
      <td>16</td>
    </tr>
    <tr>
      <th>15</th>
      <td>강서구</td>
      <td>108</td>
      <td>13</td>
      <td>1649</td>
      <td>685</td>
      <td>24</td>
      <td>92558</td>
      <td>4</td>
      <td>20</td>
      <td>22</td>
    </tr>
    <tr>
      <th>16</th>
      <td>구로구</td>
      <td>76</td>
      <td>3</td>
      <td>1420</td>
      <td>509</td>
      <td>27</td>
      <td>72611</td>
      <td>2</td>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <th>17</th>
      <td>금천구</td>
      <td>41</td>
      <td>4</td>
      <td>848</td>
      <td>276</td>
      <td>12</td>
      <td>41041</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>영등포구</td>
      <td>74</td>
      <td>4</td>
      <td>1714</td>
      <td>607</td>
      <td>30</td>
      <td>62516</td>
      <td>2</td>
      <td>10</td>
      <td>12</td>
    </tr>
    <tr>
      <th>19</th>
      <td>동작구</td>
      <td>58</td>
      <td>8</td>
      <td>748</td>
      <td>307</td>
      <td>40</td>
      <td>66613</td>
      <td>4</td>
      <td>24</td>
      <td>28</td>
    </tr>
    <tr>
      <th>20</th>
      <td>관악구</td>
      <td>75</td>
      <td>10</td>
      <td>840</td>
      <td>453</td>
      <td>19</td>
      <td>79871</td>
      <td>4</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>21</th>
      <td>서초구</td>
      <td>45</td>
      <td>9</td>
      <td>1564</td>
      <td>758</td>
      <td>36</td>
      <td>60678</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>강남구</td>
      <td>78</td>
      <td>13</td>
      <td>2062</td>
      <td>895</td>
      <td>21</td>
      <td>78226</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>송파구</td>
      <td>92</td>
      <td>12</td>
      <td>1931</td>
      <td>876</td>
      <td>4</td>
      <td>97691</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>24</th>
      <td>강동구</td>
      <td>69</td>
      <td>9</td>
      <td>1675</td>
      <td>558</td>
      <td>25</td>
      <td>74070</td>
      <td>3</td>
      <td>15</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
dat.to_csv('최종데이터.csv', index = False)
```


```python
dat = pd.read_csv("최종데이터.csv")
```

# 회귀분석


```python
dat = pd.read_csv("최종데이터2.csv")
#데이터 분석 도중 추가로 신호등 있는 횡단보도와 없는 횡단보도의 데이터를 가져옴
dat['사고비율'] = dat['사고건수']/dat['노인인구수']
print(dat)
#행정구 이름 제거
dat1 = dat[['사고건수','사고비율','노인보호구역수','횡단보도수','신호등있','신호등없','도로연장횡단','잔여시간표시기','보행작동신호기',
        '노인인구수','사고다발구역','다발구역건수','다발구역사상자수']]
dat1.head()
```

         행정구  사고건수  노인보호구역수  횡단보도수  신호등있  신호등없  도로연장횡단  잔여시간표시기  보행작동신호기  노인인구수  \
    0    종로구    43        5   1094   313   781    3.97      263       30  27818   
    1     중구    43        8   1366   318  1048   11.96      288       29  24392   
    2    용산구    32        5   1066   330   736    3.79      246       42  39070   
    3    성동구    54        5   1170   349   821    2.87      386       31  46380   
    4    광진구    40        5   1008   313   695    3.10      301       22  51723   
    5   동대문구   109        0   1361   428   933    4.15      410       46  62211   
    6    중랑구    67        3   1544   431  1113    4.70      351       36  71682   
    7    성북구    75        8   1861   496  1365    2.94      424       42  74709   
    8    강북구    76        2   1264   349   915    5.55      316       94  64333   
    9    도봉구    61        2    988   362   626    3.83      298       74  64160   
    10   노원구    79        6   1546   599   947    5.35      582       38  88345   
    11   은평구    73       10   1512   519   993    4.21      374       18  87241   
    12  서대문구    66        6   1128   343   785    3.62      298       30  54268   
    13   마포구    39        6   1930   599  1331    4.57      481       42  54582   
    14   양천구    71        7   1694   567  1127    4.12      552       51  68627   
    15   강서구   108       13   2026   806  1220    5.00      635       24  92558   
    16   구로구    76        3   1679   549  1130    5.65      509       27  72611   
    17   금천구    41        4    916   331   585    4.88      276       12  41041   
    18  영등포구    74        4   2150   733  1417    5.41      607       30  62516   
    19   동작구    58        8   1079   281   798    4.31      257       40  66613   
    20   관악구    75       10    975   348   627    3.03      303       19  79871   
    21   서초구    45        9   2028   584  1444    5.25      558       36  60678   
    22   강남구    78       13   2688   689  1999    6.09      545       21  78226   
    23   송파구    92       12   2185   758  1427    6.05      678        4  97691   
    24   강동구    69        9   1626   572  1054    5.00      558       25  74070   
    
        사고다발구역  다발구역건수  다발구역사상자수      사고비율  
    0        1       5         5  0.001546  
    1        3      16        16  0.001763  
    2        1       5         5  0.000819  
    3        0       0         0  0.001164  
    4        0       0         0  0.000773  
    5        7      46        50  0.001752  
    6        1       5         5  0.000935  
    7        5      26        27  0.001004  
    8        5      25        27  0.001181  
    9        3      15        15  0.000951  
    10       3      15        18  0.000894  
    11       5      25        26  0.000837  
    12       5      25        28  0.001216  
    13       1       5         5  0.000715  
    14       3      15        16  0.001035  
    15       4      20        22  0.001167  
    16       2      10        10  0.001047  
    17       0       0         0  0.000999  
    18       2      10        12  0.001184  
    19       4      24        28  0.000871  
    20       4      20        20  0.000939  
    21       0       0         0  0.000742  
    22       0       0         0  0.000997  
    23       1       5         5  0.000942  
    24       3      15        17  0.000932  





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
      <th>사고건수</th>
      <th>사고비율</th>
      <th>노인보호구역수</th>
      <th>횡단보도수</th>
      <th>신호등있</th>
      <th>신호등없</th>
      <th>도로연장횡단</th>
      <th>잔여시간표시기</th>
      <th>보행작동신호기</th>
      <th>노인인구수</th>
      <th>사고다발구역</th>
      <th>다발구역건수</th>
      <th>다발구역사상자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>43</td>
      <td>0.001546</td>
      <td>5</td>
      <td>1094</td>
      <td>313</td>
      <td>781</td>
      <td>3.97</td>
      <td>263</td>
      <td>30</td>
      <td>27818</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>43</td>
      <td>0.001763</td>
      <td>8</td>
      <td>1366</td>
      <td>318</td>
      <td>1048</td>
      <td>11.96</td>
      <td>288</td>
      <td>29</td>
      <td>24392</td>
      <td>3</td>
      <td>16</td>
      <td>16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>32</td>
      <td>0.000819</td>
      <td>5</td>
      <td>1066</td>
      <td>330</td>
      <td>736</td>
      <td>3.79</td>
      <td>246</td>
      <td>42</td>
      <td>39070</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>54</td>
      <td>0.001164</td>
      <td>5</td>
      <td>1170</td>
      <td>349</td>
      <td>821</td>
      <td>2.87</td>
      <td>386</td>
      <td>31</td>
      <td>46380</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>40</td>
      <td>0.000773</td>
      <td>5</td>
      <td>1008</td>
      <td>313</td>
      <td>695</td>
      <td>3.10</td>
      <td>301</td>
      <td>22</td>
      <td>51723</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 분포 확인
dat1.describe()
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
      <th>사고건수</th>
      <th>사고비율</th>
      <th>노인보호구역수</th>
      <th>횡단보도수</th>
      <th>신호등있</th>
      <th>신호등없</th>
      <th>도로연장횡단</th>
      <th>잔여시간표시기</th>
      <th>보행작동신호기</th>
      <th>노인인구수</th>
      <th>사고다발구역</th>
      <th>다발구역건수</th>
      <th>다발구역사상자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
      <td>25.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>65.760000</td>
      <td>0.001056</td>
      <td>6.520000</td>
      <td>1515.360000</td>
      <td>478.680000</td>
      <td>1036.680000</td>
      <td>4.776000</td>
      <td>419.840000</td>
      <td>34.520000</td>
      <td>64216.640000</td>
      <td>2.520000</td>
      <td>13.280000</td>
      <td>14.280000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>20.441543</td>
      <td>0.000277</td>
      <td>3.465545</td>
      <td>467.670636</td>
      <td>159.266998</td>
      <td>332.633272</td>
      <td>1.773791</td>
      <td>137.388949</td>
      <td>18.672886</td>
      <td>18931.645977</td>
      <td>2.002498</td>
      <td>11.392688</td>
      <td>12.428194</td>
    </tr>
    <tr>
      <th>min</th>
      <td>32.000000</td>
      <td>0.000715</td>
      <td>0.000000</td>
      <td>916.000000</td>
      <td>281.000000</td>
      <td>585.000000</td>
      <td>2.870000</td>
      <td>246.000000</td>
      <td>4.000000</td>
      <td>24392.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>45.000000</td>
      <td>0.000894</td>
      <td>4.000000</td>
      <td>1094.000000</td>
      <td>343.000000</td>
      <td>785.000000</td>
      <td>3.830000</td>
      <td>298.000000</td>
      <td>24.000000</td>
      <td>54268.000000</td>
      <td>1.000000</td>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>69.000000</td>
      <td>0.000997</td>
      <td>6.000000</td>
      <td>1512.000000</td>
      <td>431.000000</td>
      <td>993.000000</td>
      <td>4.570000</td>
      <td>386.000000</td>
      <td>30.000000</td>
      <td>64333.000000</td>
      <td>3.000000</td>
      <td>15.000000</td>
      <td>15.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>76.000000</td>
      <td>0.001167</td>
      <td>9.000000</td>
      <td>1861.000000</td>
      <td>584.000000</td>
      <td>1220.000000</td>
      <td>5.350000</td>
      <td>552.000000</td>
      <td>42.000000</td>
      <td>74709.000000</td>
      <td>4.000000</td>
      <td>20.000000</td>
      <td>22.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>109.000000</td>
      <td>0.001763</td>
      <td>13.000000</td>
      <td>2688.000000</td>
      <td>806.000000</td>
      <td>1999.000000</td>
      <td>11.960000</td>
      <td>678.000000</td>
      <td>94.000000</td>
      <td>97691.000000</td>
      <td>7.000000</td>
      <td>46.000000</td>
      <td>50.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#산점도 확인
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
col_name = ['사고건수','사고비율','노인보호구역수','횡단보도수',
            '신호등있','신호등없','도로연장횡단','잔여시간표시기','보행작동신호기','노인인구수',
            '사고다발구역','다발구역건수','다발구역사상자수']

sns.pairplot(dat[col_name])
plt.show()
```







```python
#아웃라이어 제거를 위해 함수 설정
import numpy as np

def outlier_iqr(data, column): 

    global lower, upper    
    
    q25, q75 = np.quantile(data[column], 0.25), np.quantile(data[column], 0.75)          
    
    iqr = q75 - q25    
    
    cut_off = iqr * 1.5          
    
    lower, upper = q25 - cut_off, q75 + cut_off     
    
    print('IQR은',iqr, '이다.')     
    print('lower bound 값은', lower, '이다.')     
    print('upper bound 값은', upper, '이다.')    
    
    data1 = data[data[column] > upper]     
    data2 = data[data[column] < lower]    
    
    return print('총 이상치 개수는', data1.shape[0] + data2.shape[0], '이다.')
```


```python
outlier_iqr(dat, '보행작동신호기')

dat1 = dat.drop([8,9])
dat1
```

    IQR은 18.0 이다.
    lower bound 값은 -3.0 이다.
    upper bound 값은 69.0 이다.
    총 이상치 개수는 2 이다.





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
      <th>행정구</th>
      <th>사고건수</th>
      <th>노인보호구역수</th>
      <th>횡단보도수</th>
      <th>신호등있</th>
      <th>신호등없</th>
      <th>도로연장횡단</th>
      <th>잔여시간표시기</th>
      <th>보행작동신호기</th>
      <th>노인인구수</th>
      <th>사고다발구역</th>
      <th>다발구역건수</th>
      <th>다발구역사상자수</th>
      <th>사고비율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>종로구</td>
      <td>43</td>
      <td>5</td>
      <td>1094</td>
      <td>313</td>
      <td>781</td>
      <td>3.97</td>
      <td>263</td>
      <td>30</td>
      <td>27818</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>0.001546</td>
    </tr>
    <tr>
      <th>1</th>
      <td>중구</td>
      <td>43</td>
      <td>8</td>
      <td>1366</td>
      <td>318</td>
      <td>1048</td>
      <td>11.96</td>
      <td>288</td>
      <td>29</td>
      <td>24392</td>
      <td>3</td>
      <td>16</td>
      <td>16</td>
      <td>0.001763</td>
    </tr>
    <tr>
      <th>2</th>
      <td>용산구</td>
      <td>32</td>
      <td>5</td>
      <td>1066</td>
      <td>330</td>
      <td>736</td>
      <td>3.79</td>
      <td>246</td>
      <td>42</td>
      <td>39070</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>0.000819</td>
    </tr>
    <tr>
      <th>3</th>
      <td>성동구</td>
      <td>54</td>
      <td>5</td>
      <td>1170</td>
      <td>349</td>
      <td>821</td>
      <td>2.87</td>
      <td>386</td>
      <td>31</td>
      <td>46380</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.001164</td>
    </tr>
    <tr>
      <th>4</th>
      <td>광진구</td>
      <td>40</td>
      <td>5</td>
      <td>1008</td>
      <td>313</td>
      <td>695</td>
      <td>3.10</td>
      <td>301</td>
      <td>22</td>
      <td>51723</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.000773</td>
    </tr>
    <tr>
      <th>5</th>
      <td>동대문구</td>
      <td>109</td>
      <td>0</td>
      <td>1361</td>
      <td>428</td>
      <td>933</td>
      <td>4.15</td>
      <td>410</td>
      <td>46</td>
      <td>62211</td>
      <td>7</td>
      <td>46</td>
      <td>50</td>
      <td>0.001752</td>
    </tr>
    <tr>
      <th>6</th>
      <td>중랑구</td>
      <td>67</td>
      <td>3</td>
      <td>1544</td>
      <td>431</td>
      <td>1113</td>
      <td>4.70</td>
      <td>351</td>
      <td>36</td>
      <td>71682</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>0.000935</td>
    </tr>
    <tr>
      <th>7</th>
      <td>성북구</td>
      <td>75</td>
      <td>8</td>
      <td>1861</td>
      <td>496</td>
      <td>1365</td>
      <td>2.94</td>
      <td>424</td>
      <td>42</td>
      <td>74709</td>
      <td>5</td>
      <td>26</td>
      <td>27</td>
      <td>0.001004</td>
    </tr>
    <tr>
      <th>10</th>
      <td>노원구</td>
      <td>79</td>
      <td>6</td>
      <td>1546</td>
      <td>599</td>
      <td>947</td>
      <td>5.35</td>
      <td>582</td>
      <td>38</td>
      <td>88345</td>
      <td>3</td>
      <td>15</td>
      <td>18</td>
      <td>0.000894</td>
    </tr>
    <tr>
      <th>11</th>
      <td>은평구</td>
      <td>73</td>
      <td>10</td>
      <td>1512</td>
      <td>519</td>
      <td>993</td>
      <td>4.21</td>
      <td>374</td>
      <td>18</td>
      <td>87241</td>
      <td>5</td>
      <td>25</td>
      <td>26</td>
      <td>0.000837</td>
    </tr>
    <tr>
      <th>12</th>
      <td>서대문구</td>
      <td>66</td>
      <td>6</td>
      <td>1128</td>
      <td>343</td>
      <td>785</td>
      <td>3.62</td>
      <td>298</td>
      <td>30</td>
      <td>54268</td>
      <td>5</td>
      <td>25</td>
      <td>28</td>
      <td>0.001216</td>
    </tr>
    <tr>
      <th>13</th>
      <td>마포구</td>
      <td>39</td>
      <td>6</td>
      <td>1930</td>
      <td>599</td>
      <td>1331</td>
      <td>4.57</td>
      <td>481</td>
      <td>42</td>
      <td>54582</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>0.000715</td>
    </tr>
    <tr>
      <th>14</th>
      <td>양천구</td>
      <td>71</td>
      <td>7</td>
      <td>1694</td>
      <td>567</td>
      <td>1127</td>
      <td>4.12</td>
      <td>552</td>
      <td>51</td>
      <td>68627</td>
      <td>3</td>
      <td>15</td>
      <td>16</td>
      <td>0.001035</td>
    </tr>
    <tr>
      <th>15</th>
      <td>강서구</td>
      <td>108</td>
      <td>13</td>
      <td>2026</td>
      <td>806</td>
      <td>1220</td>
      <td>5.00</td>
      <td>635</td>
      <td>24</td>
      <td>92558</td>
      <td>4</td>
      <td>20</td>
      <td>22</td>
      <td>0.001167</td>
    </tr>
    <tr>
      <th>16</th>
      <td>구로구</td>
      <td>76</td>
      <td>3</td>
      <td>1679</td>
      <td>549</td>
      <td>1130</td>
      <td>5.65</td>
      <td>509</td>
      <td>27</td>
      <td>72611</td>
      <td>2</td>
      <td>10</td>
      <td>10</td>
      <td>0.001047</td>
    </tr>
    <tr>
      <th>17</th>
      <td>금천구</td>
      <td>41</td>
      <td>4</td>
      <td>916</td>
      <td>331</td>
      <td>585</td>
      <td>4.88</td>
      <td>276</td>
      <td>12</td>
      <td>41041</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.000999</td>
    </tr>
    <tr>
      <th>18</th>
      <td>영등포구</td>
      <td>74</td>
      <td>4</td>
      <td>2150</td>
      <td>733</td>
      <td>1417</td>
      <td>5.41</td>
      <td>607</td>
      <td>30</td>
      <td>62516</td>
      <td>2</td>
      <td>10</td>
      <td>12</td>
      <td>0.001184</td>
    </tr>
    <tr>
      <th>19</th>
      <td>동작구</td>
      <td>58</td>
      <td>8</td>
      <td>1079</td>
      <td>281</td>
      <td>798</td>
      <td>4.31</td>
      <td>257</td>
      <td>40</td>
      <td>66613</td>
      <td>4</td>
      <td>24</td>
      <td>28</td>
      <td>0.000871</td>
    </tr>
    <tr>
      <th>20</th>
      <td>관악구</td>
      <td>75</td>
      <td>10</td>
      <td>975</td>
      <td>348</td>
      <td>627</td>
      <td>3.03</td>
      <td>303</td>
      <td>19</td>
      <td>79871</td>
      <td>4</td>
      <td>20</td>
      <td>20</td>
      <td>0.000939</td>
    </tr>
    <tr>
      <th>21</th>
      <td>서초구</td>
      <td>45</td>
      <td>9</td>
      <td>2028</td>
      <td>584</td>
      <td>1444</td>
      <td>5.25</td>
      <td>558</td>
      <td>36</td>
      <td>60678</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.000742</td>
    </tr>
    <tr>
      <th>22</th>
      <td>강남구</td>
      <td>78</td>
      <td>13</td>
      <td>2688</td>
      <td>689</td>
      <td>1999</td>
      <td>6.09</td>
      <td>545</td>
      <td>21</td>
      <td>78226</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.000997</td>
    </tr>
    <tr>
      <th>23</th>
      <td>송파구</td>
      <td>92</td>
      <td>12</td>
      <td>2185</td>
      <td>758</td>
      <td>1427</td>
      <td>6.05</td>
      <td>678</td>
      <td>4</td>
      <td>97691</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>0.000942</td>
    </tr>
    <tr>
      <th>24</th>
      <td>강동구</td>
      <td>69</td>
      <td>9</td>
      <td>1626</td>
      <td>572</td>
      <td>1054</td>
      <td>5.00</td>
      <td>558</td>
      <td>25</td>
      <td>74070</td>
      <td>3</td>
      <td>15</td>
      <td>17</td>
      <td>0.000932</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Min-max 스케일링을 진행

from sklearn.preprocessing import MinMaxScaler

min_max_scaler = MinMaxScaler()

fitted = min_max_scaler.fit(dat1)

fitted = min_max_scaler.transform(dat1)

dat_scaler = dat1[:0]
for i in range(len(fitted)):
    dat_scaler.loc[i] = fitted[i]

dat_scaler

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
      <th>사고건수</th>
      <th>사고비율</th>
      <th>노인보호구역수</th>
      <th>횡단보도수</th>
      <th>신호등있</th>
      <th>신호등없</th>
      <th>도로연장횡단</th>
      <th>잔여시간표시기</th>
      <th>보행작동신호기</th>
      <th>노인인구수</th>
      <th>사고다발구역</th>
      <th>다발구역건수</th>
      <th>다발구역사상자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.142857</td>
      <td>0.792902</td>
      <td>0.384615</td>
      <td>0.100451</td>
      <td>0.060952</td>
      <td>0.138614</td>
      <td>0.121012</td>
      <td>0.039352</td>
      <td>0.288889</td>
      <td>0.046740</td>
      <td>0.142857</td>
      <td>0.108696</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.142857</td>
      <td>1.000000</td>
      <td>0.615385</td>
      <td>0.253950</td>
      <td>0.070476</td>
      <td>0.327440</td>
      <td>1.000000</td>
      <td>0.097222</td>
      <td>0.277778</td>
      <td>0.000000</td>
      <td>0.428571</td>
      <td>0.347826</td>
      <td>0.32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.000000</td>
      <td>0.099701</td>
      <td>0.384615</td>
      <td>0.084650</td>
      <td>0.093333</td>
      <td>0.106789</td>
      <td>0.101210</td>
      <td>0.000000</td>
      <td>0.422222</td>
      <td>0.200248</td>
      <td>0.142857</td>
      <td>0.108696</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.285714</td>
      <td>0.429029</td>
      <td>0.384615</td>
      <td>0.143341</td>
      <td>0.129524</td>
      <td>0.166902</td>
      <td>0.000000</td>
      <td>0.324074</td>
      <td>0.300000</td>
      <td>0.299977</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.103896</td>
      <td>0.056116</td>
      <td>0.384615</td>
      <td>0.051919</td>
      <td>0.060952</td>
      <td>0.077793</td>
      <td>0.025303</td>
      <td>0.127315</td>
      <td>0.200000</td>
      <td>0.372870</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1.000000</td>
      <td>0.989725</td>
      <td>0.000000</td>
      <td>0.251129</td>
      <td>0.280000</td>
      <td>0.246110</td>
      <td>0.140814</td>
      <td>0.379630</td>
      <td>0.466667</td>
      <td>0.515955</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.454545</td>
      <td>0.210008</td>
      <td>0.230769</td>
      <td>0.354402</td>
      <td>0.285714</td>
      <td>0.373409</td>
      <td>0.201320</td>
      <td>0.243056</td>
      <td>0.355556</td>
      <td>0.645166</td>
      <td>0.142857</td>
      <td>0.108696</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.558442</td>
      <td>0.276027</td>
      <td>0.615385</td>
      <td>0.533296</td>
      <td>0.409524</td>
      <td>0.551627</td>
      <td>0.007701</td>
      <td>0.412037</td>
      <td>0.422222</td>
      <td>0.686462</td>
      <td>0.714286</td>
      <td>0.565217</td>
      <td>0.54</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.571429</td>
      <td>0.445301</td>
      <td>0.153846</td>
      <td>0.196388</td>
      <td>0.129524</td>
      <td>0.233380</td>
      <td>0.294829</td>
      <td>0.162037</td>
      <td>1.000000</td>
      <td>0.544905</td>
      <td>0.714286</td>
      <td>0.543478</td>
      <td>0.54</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.376623</td>
      <td>0.225332</td>
      <td>0.153846</td>
      <td>0.040632</td>
      <td>0.154286</td>
      <td>0.028996</td>
      <td>0.105611</td>
      <td>0.120370</td>
      <td>0.777778</td>
      <td>0.542545</td>
      <td>0.428571</td>
      <td>0.326087</td>
      <td>0.30</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.610390</td>
      <td>0.171412</td>
      <td>0.461538</td>
      <td>0.355530</td>
      <td>0.605714</td>
      <td>0.256011</td>
      <td>0.272827</td>
      <td>0.777778</td>
      <td>0.377778</td>
      <td>0.872495</td>
      <td>0.428571</td>
      <td>0.326087</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.532468</td>
      <td>0.116603</td>
      <td>0.769231</td>
      <td>0.336343</td>
      <td>0.453333</td>
      <td>0.288543</td>
      <td>0.147415</td>
      <td>0.296296</td>
      <td>0.155556</td>
      <td>0.857433</td>
      <td>0.714286</td>
      <td>0.543478</td>
      <td>0.52</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.441558</td>
      <td>0.478527</td>
      <td>0.461538</td>
      <td>0.119639</td>
      <td>0.118095</td>
      <td>0.141443</td>
      <td>0.082508</td>
      <td>0.120370</td>
      <td>0.288889</td>
      <td>0.407591</td>
      <td>0.714286</td>
      <td>0.543478</td>
      <td>0.56</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0.090909</td>
      <td>0.000000</td>
      <td>0.461538</td>
      <td>0.572235</td>
      <td>0.605714</td>
      <td>0.527581</td>
      <td>0.187019</td>
      <td>0.543981</td>
      <td>0.422222</td>
      <td>0.411875</td>
      <td>0.142857</td>
      <td>0.108696</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.506494</td>
      <td>0.305295</td>
      <td>0.538462</td>
      <td>0.439052</td>
      <td>0.544762</td>
      <td>0.383310</td>
      <td>0.137514</td>
      <td>0.708333</td>
      <td>0.522222</td>
      <td>0.603487</td>
      <td>0.428571</td>
      <td>0.326087</td>
      <td>0.32</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.987013</td>
      <td>0.431453</td>
      <td>1.000000</td>
      <td>0.626411</td>
      <td>1.000000</td>
      <td>0.449081</td>
      <td>0.234323</td>
      <td>0.900463</td>
      <td>0.222222</td>
      <td>0.929972</td>
      <td>0.571429</td>
      <td>0.434783</td>
      <td>0.44</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.571429</td>
      <td>0.316833</td>
      <td>0.230769</td>
      <td>0.430587</td>
      <td>0.510476</td>
      <td>0.385431</td>
      <td>0.305831</td>
      <td>0.608796</td>
      <td>0.255556</td>
      <td>0.657840</td>
      <td>0.285714</td>
      <td>0.217391</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>17</th>
      <td>0.116883</td>
      <td>0.271359</td>
      <td>0.307692</td>
      <td>0.000000</td>
      <td>0.095238</td>
      <td>0.000000</td>
      <td>0.221122</td>
      <td>0.069444</td>
      <td>0.088889</td>
      <td>0.227138</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.545455</td>
      <td>0.447537</td>
      <td>0.307692</td>
      <td>0.696388</td>
      <td>0.860952</td>
      <td>0.588402</td>
      <td>0.279428</td>
      <td>0.835648</td>
      <td>0.288889</td>
      <td>0.520116</td>
      <td>0.285714</td>
      <td>0.217391</td>
      <td>0.24</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.337662</td>
      <td>0.148976</td>
      <td>0.615385</td>
      <td>0.091986</td>
      <td>0.000000</td>
      <td>0.150636</td>
      <td>0.158416</td>
      <td>0.025463</td>
      <td>0.400000</td>
      <td>0.576011</td>
      <td>0.571429</td>
      <td>0.521739</td>
      <td>0.56</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.558442</td>
      <td>0.214139</td>
      <td>0.769231</td>
      <td>0.033296</td>
      <td>0.127619</td>
      <td>0.029703</td>
      <td>0.017602</td>
      <td>0.131944</td>
      <td>0.166667</td>
      <td>0.756886</td>
      <td>0.571429</td>
      <td>0.434783</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0.168831</td>
      <td>0.025849</td>
      <td>0.692308</td>
      <td>0.627540</td>
      <td>0.577143</td>
      <td>0.607496</td>
      <td>0.261826</td>
      <td>0.722222</td>
      <td>0.355556</td>
      <td>0.495041</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.597403</td>
      <td>0.269556</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.777143</td>
      <td>1.000000</td>
      <td>0.354235</td>
      <td>0.692130</td>
      <td>0.188889</td>
      <td>0.734444</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.779221</td>
      <td>0.216744</td>
      <td>0.923077</td>
      <td>0.716140</td>
      <td>0.908571</td>
      <td>0.595474</td>
      <td>0.349835</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.142857</td>
      <td>0.108696</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.480519</td>
      <td>0.207020</td>
      <td>0.692308</td>
      <td>0.400677</td>
      <td>0.554286</td>
      <td>0.331683</td>
      <td>0.234323</td>
      <td>0.722222</td>
      <td>0.233333</td>
      <td>0.677745</td>
      <td>0.428571</td>
      <td>0.326087</td>
      <td>0.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
col_name = ['사고건수','사고비율','노인보호구역수','횡단보도수',
            '신호등있','신호등없','도로연장횡단','잔여시간표시기','보행작동신호기','노인인구수',
            '사고다발구역','다발구역건수','다발구역사상자수']

sns.pairplot(dat[col_name])
plt.show()
```






```python
#상관계수를 확인해봄
#사고건수에 대한 상관계수가 이상하여 사고 비율이라는 변수를 추가해봄

from sklearn.datasets import load_boston
from matplotlib import pyplot as plt
import seaborn as sns
import pandas as pd
import numpy as np
from matplotlib import font_manager, rc

font_path = "C:/Windows/Fonts/gulim.ttc"
font = font_manager.FontProperties(fname = font_path).get_name()
rc('font', family = font)



col_name = ['사고건수','사고비율','노인보호구역수','횡단보도수',
            '신호등있','신호등없','도로연장횡단','잔여시간표시기','보행작동신호기','노인인구수',
            '사고다발구역','다발구역건수','다발구역사상자수']

dfX = pd.DataFrame(dat_scaler, columns = col_name)

cmap = sns.light_palette('black',as_cmap = True)
sns.heatmap(dfX.corr(), annot = True, fmt = '3.2f', cmap = cmap)
```




    <AxesSubplot:>


​    


# 다중 공선성 확인


```python
## VIF 결과 행정구가 dtype이 달라서 빼고 계산. 범주형으로 넣을것인지
import pandas as pd
import statsmodels.api as sm
from statsmodels.stats.outliers_influence import variance_inflation_factor

col_name = ['노인보호구역수','횡단보도수',
            '신호등있','신호등없','도로연장횡단','잔여시간표시기','보행작동신호기','노인인구수',
            '사고다발구역','다발구역건수','다발구역사상자수']

x_train = dat_scaler[col_name]

def feature_engineering_XbyVIF(x_train):
    vif = pd.DataFrame()
    vif['VIF_Factor'] = [variance_inflation_factor(x_train.values, i)
                        for i in range(x_train.shape[1])]
    vif['Feature'] = x_train.columns
    return vif

vif = feature_engineering_XbyVIF(x_train)
print(vif)

#신호등, 횡단보도수와 다발구역 관련이 높게 나타남
```

         VIF_Factor   Feature
    0      2.823811   노인보호구역수
    1   4523.146993     횡단보도수
    2    559.601979      신호등있
    3   2532.536721      신호등없
    4      1.425179    도로연장횡단
    5     13.296082   잔여시간표시기
    6      2.178692   보행작동신호기
    7      3.283490     노인인구수
    8     58.329263    사고다발구역
    9    319.861225    다발구역건수
    10   225.172485  다발구역사상자수



```python
col_name = ['노인보호구역수'
            ,'도로연장횡단','잔여시간표시기','보행작동신호기',
            '사고다발구역']

x_train = dat_scaler[col_name]

def feature_engineering_XbyVIF(x_train):
    vif = pd.DataFrame()
    vif['VIF_Factor'] = [variance_inflation_factor(x_train.values, i)
                        for i in range(x_train.shape[1])]
    vif['Feature'] = x_train.columns
    return vif

vif = feature_engineering_XbyVIF(x_train)
print(vif)
#다중공선성 제거후 최종 선택 데이터
```

       VIF_Factor  Feature
    0    4.229494  노인보호구역수
    1    2.227175   도로연장횡단
    2    3.272614  잔여시간표시기
    3    3.016120  보행작동신호기
    4    3.072518   사고다발구역



```python
col_name = ['노인보호구역수','도로연장횡단','잔여시간표시기','보행작동신호기',
            '사고다발구역']
dat_scaler['intercept'] = 1 # 필요시 추가 제거
col_name.append('intercept') 

model = sm.OLS(dat_scaler['사고비율'],dat_scaler[col_name])

results = model.fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   사고비율   R-squared:                       0.491
    Model:                            OLS   Adj. R-squared:                  0.357
    Method:                 Least Squares   F-statistic:                     3.663
    Date:                Fri, 02 Dec 2022   Prob (F-statistic):             0.0173
    Time:                        20:29:53   Log-Likelihood:                 6.7322
    No. Observations:                  25   AIC:                            -1.464
    Df Residuals:                      19   BIC:                             5.849
    Df Model:                           5                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    노인보호구역수       -0.4317      0.212     -2.032      0.056      -0.876       0.013
    도로연장횡단         0.6939      0.227      3.063      0.006       0.220       1.168
    잔여시간표시기       -0.0729      0.152     -0.480      0.637      -0.391       0.245
    보행작동신호기       -0.3674      0.268     -1.371      0.186      -0.928       0.193
    사고다발구역         0.4202      0.164      2.568      0.019       0.078       0.763
    intercept      0.3995      0.171      2.341      0.030       0.042       0.757
    ==============================================================================
    Omnibus:                        6.691   Durbin-Watson:                   1.855
    Prob(Omnibus):                  0.035   Jarque-Bera (JB):                4.774
    Skew:                           1.012   Prob(JB):                       0.0919
    Kurtosis:                       3.698   Cond. No.                         10.5
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



```python
import statsmodels.api as sm

#회귀모형 풀모델을 돌렸을때 제거한후 최종 모델

col_name = ['노인보호구역수','도로연장횡단',
            '사고다발구역']
dat_scaler['intercept'] = 1 # 필요시 추가 제거
col_name.append('intercept') 

model = sm.OLS(dat_scaler['사고비율'],dat_scaler[col_name])

results = model.fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   사고비율   R-squared:                       0.471
    Model:                            OLS   Adj. R-squared:                  0.388
    Method:                 Least Squares   F-statistic:                     5.645
    Date:                Fri, 02 Dec 2022   Prob (F-statistic):            0.00612
    Time:                        15:10:14   Log-Likelihood:                 4.9695
    No. Observations:                  23   AIC:                            -1.939
    Df Residuals:                      19   BIC:                             2.603
    Df Model:                           3                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    노인보호구역수       -0.4088      0.182     -2.248      0.037      -0.790      -0.028
    도로연장횡단         0.6997      0.232      3.022      0.007       0.215       1.184
    사고다발구역         0.3862      0.159      2.426      0.025       0.053       0.719
    intercept      0.2632      0.123      2.132      0.046       0.005       0.522
    ==============================================================================
    Omnibus:                        5.685   Durbin-Watson:                   1.878
    Prob(Omnibus):                  0.058   Jarque-Bera (JB):                3.714
    Skew:                           0.929   Prob(JB):                        0.156
    Kurtosis:                       3.649   Cond. No.                         6.46
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


# 모델 스코어


```python
score = pd.DataFrame()
city = ['종로구','중구','용산구','성동구','광진구','동대문구','중랑구','성북구','노원구','은평구','서대문구','마포구','양천구','강서구','구로구','금천구','영등포구','동작구','관악구','서초구','강남구','송파구','강동구']

score['행정구역'] = city
score['스코어'] = -0.4088*dat_scaler['노인보호구역수'] + 0.6997*dat_scaler['도로연장횡단'] + 0.3862 * dat_scaler['사고다발구역']
score['노인보호구역'] = dat_scaler['노인보호구역수']
score['도로연장횡단'] = dat_scaler['도로연장횡단']
score['사고다발구역'] = dat_scaler['사고다발구역']
score
#스코어는 행정구역에서 노인사고발생률에 대한 스코어를 의미한다

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
      <th>행정구역</th>
      <th>스코어</th>
      <th>노인보호구역</th>
      <th>도로연장횡단</th>
      <th>사고다발구역</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>종로구</td>
      <td>-0.017387</td>
      <td>0.384615</td>
      <td>0.121012</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>1</th>
      <td>중구</td>
      <td>0.613645</td>
      <td>0.615385</td>
      <td>1.000000</td>
      <td>0.428571</td>
    </tr>
    <tr>
      <th>2</th>
      <td>용산구</td>
      <td>-0.031243</td>
      <td>0.384615</td>
      <td>0.101210</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>3</th>
      <td>성동구</td>
      <td>-0.157231</td>
      <td>0.384615</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>광진구</td>
      <td>-0.139527</td>
      <td>0.384615</td>
      <td>0.025303</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>동대문구</td>
      <td>0.484728</td>
      <td>0.000000</td>
      <td>0.140814</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>중랑구</td>
      <td>0.101697</td>
      <td>0.230769</td>
      <td>0.201320</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>7</th>
      <td>성북구</td>
      <td>0.029676</td>
      <td>0.615385</td>
      <td>0.007701</td>
      <td>0.714286</td>
    </tr>
    <tr>
      <th>8</th>
      <td>노원구</td>
      <td>0.419257</td>
      <td>0.153846</td>
      <td>0.294829</td>
      <td>0.714286</td>
    </tr>
    <tr>
      <th>9</th>
      <td>은평구</td>
      <td>0.176518</td>
      <td>0.153846</td>
      <td>0.105611</td>
      <td>0.428571</td>
    </tr>
    <tr>
      <th>10</th>
      <td>서대문구</td>
      <td>0.167735</td>
      <td>0.461538</td>
      <td>0.272827</td>
      <td>0.428571</td>
    </tr>
    <tr>
      <th>11</th>
      <td>마포구</td>
      <td>0.064542</td>
      <td>0.769231</td>
      <td>0.147415</td>
      <td>0.714286</td>
    </tr>
    <tr>
      <th>12</th>
      <td>양천구</td>
      <td>0.144911</td>
      <td>0.461538</td>
      <td>0.082508</td>
      <td>0.714286</td>
    </tr>
    <tr>
      <th>13</th>
      <td>강서구</td>
      <td>-0.002649</td>
      <td>0.461538</td>
      <td>0.187019</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>14</th>
      <td>구로구</td>
      <td>0.041610</td>
      <td>0.538462</td>
      <td>0.137514</td>
      <td>0.428571</td>
    </tr>
    <tr>
      <th>15</th>
      <td>금천구</td>
      <td>-0.024158</td>
      <td>1.000000</td>
      <td>0.234323</td>
      <td>0.571429</td>
    </tr>
    <tr>
      <th>16</th>
      <td>영등포구</td>
      <td>0.229994</td>
      <td>0.230769</td>
      <td>0.305831</td>
      <td>0.285714</td>
    </tr>
    <tr>
      <th>17</th>
      <td>동작구</td>
      <td>0.028935</td>
      <td>0.307692</td>
      <td>0.221122</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>관악구</td>
      <td>0.180074</td>
      <td>0.307692</td>
      <td>0.279428</td>
      <td>0.285714</td>
    </tr>
    <tr>
      <th>19</th>
      <td>서초구</td>
      <td>0.079960</td>
      <td>0.615385</td>
      <td>0.158416</td>
      <td>0.571429</td>
    </tr>
    <tr>
      <th>20</th>
      <td>강남구</td>
      <td>-0.081460</td>
      <td>0.769231</td>
      <td>0.017602</td>
      <td>0.571429</td>
    </tr>
    <tr>
      <th>21</th>
      <td>송파구</td>
      <td>-0.099816</td>
      <td>0.692308</td>
      <td>0.261826</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>강동구</td>
      <td>-0.160941</td>
      <td>1.000000</td>
      <td>0.354235</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
font_path = "C:/Windows/Fonts/gulim.ttc"
font = font_manager.FontProperties(fname = font_path).get_name()
rc('font', family = font)
colors = sns.color_palette('hls',len(score['스코어']))


plt.barh(score['행정구역'],score['스코어'],color=colors)
```




    <BarContainer object of 23 artists>






```python

```
