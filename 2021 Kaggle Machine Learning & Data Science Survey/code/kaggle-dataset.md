---
title: pandas 연습1
date: 2021-11-15 16:47:57
categories:
- 캐글
- 캐글 시험 준비
tags:
- Coalb
- python
- pandas
- kaggle

---
```python

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)


import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

```


```python
df = pd.read_csv('../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv',low_memory = False)
df = df[1:].reset_index(drop=True)
```

## 데이터 프레임 편집하기 
기존 데이터 프레임에서 한중일을 따로 빼서 새로운 데이터 프레임 `df_asia` 로 만들고,
미국을 따로 뽑아서 `df_usa`로 만들기 위해 다양한 필터링 방법을 써보고 그래프까지 뽑아보자


```python
df.head()
```
![](/images/dataex05/dataex05_a.png)
- 먼저 중복되는 국가명을 제외하고 어떤 국가들이 있는지 먼저 확인해준다.


```python
df['Q3'].unique()
```
![](/images/dataex05/dataex05_b.png)
1. `drop`을 이용한 방법


```python
df=df[df.Q3 != 'India']
df['Q3'].unique()
```
![](/images/dataex05/dataex05_c.png)
- 위에 결과 처럼 기존 `df`(데이터프레임) 에서 `India`를 `drop`해서 국가에 India가 빠진걸 확인 할 수 있다.
- 이 방법을 이용해 다른 국가도 한번 빼보자


```python
df=df[df.Q3 != 'Indonesia', 'Pakistan', 'Mexico', 'Russia', 'Turkey', 'Australia',
       'Nigeria', 'Greece', 'Belgium', 'Egypt', 'Singapore',
       'Brazil', 'Poland', 'Iran, Islamic Republic of...','Italy', 'Viet Nam', 'Israel', 'Peru',
       'South Africa', 'Other', 'Spain', 'Bangladesh',
       'United Kingdom of Great Britain and Northern Ireland', 'France',
       'Switzerland', 'Algeria', 'Tunisia', 'Argentina', 'Sweden',
       'Colombia', 'I do not wish to disclose my location', 'Canada',
       'Chile', 'Netherlands', 'Ukraine', 'Saudi Arabia', 'Romania',
       'Morocco', 'Austria', 'Taiwan', 'Kenya', 'Belarus', 'Ireland',
       'Portugal', 'Hong Kong (S.A.R.)', 'Denmark', 'Germany',
       'Philippines', 'Sri Lanka', 'United Arab Emirates',
       'Uganda', 'Ghana', 'Malaysia', 'Thailand', 'Nepal', 'Kazakhstan',
       'Ethiopia', 'Iraq', 'Ecuador', 'Norway', 'Czech Republic']
```
![](/images/dataex05/dataex05_d.png)
- 많은 국가를 넣으니 키에러가 난다. ㅜㅜ 에러의 원인은 알 수 없지만 국가를 줄여서 한번 더 해본다.


```python
df=df[df.Q3 != 'Indonesia', 'Pakistan']
```
![](/images/dataex05/dataex05_e.png)

```python
df=df[df.Q3 != 'Indonesia']
df['Q3'].unique()
```
![](/images/dataex05/dataex05_f.png)
- 이유를 알 수는 없지만 1개 이상의 국가를 `drop` 할때 키에러가 발생하는거 같다.
- `drop` 을 이용한 방법은 적은 컬럼에선 가능할 것 같지만, 이 파일처럼 많은 데이터가 있을 경우 매우매우 많은 시간이 걸릴거라 생각된다.
- 위와같은 이유로 `drop`을 이용한 방법은 사용하지 못할 것 같다.

2. `isin` 을 이용한 방법
- `isin` 은 해당 데이터를 `True` or `False`로 반환해주는 기능을 한다.
- 이러한 점을 이용해 국가명을 조건으로 걸어주어서 조건에 맞는 국가만 따로 묶어본다.


```python
df_asia = df[df['Q3'].isin(['Japan', 'China', 'South Korea'])]
df_asia.head()
```
![](/images/dataex05/dataex05_g.png)

```python
df_usa = df[df['Q3'].isin(['United States of America'])]
df_usa.head()
```
![](/images/dataex05/dataex05_h.png)
- 성공이다 너무 기분이 좋다ㅎㅎㅎ
- 하지만 아직 데이터 프레임이 이쁘지 않다. 데이터를 전처리를 해야겠다.
 1. 기존 데이터 프레임에서 가져온거라 인덱스 번호가 오락가락이다 새로운 데이터프레임들의 인덱스 번호를 1부터 출력되게 바꿔주자
 2. 국가명이 문제다 `South Korea` 는 정식 명칭이 아니고 `United States of America` 는 너무 길다
   - `United States of America` 를 `USA`로 바꿔주는 김에 다른 국가들로 다 약어인 `CH`,`JAP`,`KOR` 로 변경해보자
 3. 각 질문마다 이상한 애들 (ex. 선택하지 않음, 말하기싫음 등등..)을 다 빼고 내가 원하는 데이터들만 남겨보자
 4. 질문들의 이름도 너무 제각각이다. `Q38_B_Part_4` 이런 애들을 쓰기 쉽게 바꿔보자
 
 각각의 항목은 링크로 걸겠다.

1. [인덱스 번호 재 나열](https://kimgoden.github.io/2021/11/15/kaggle-reindex/)
2. [국가명 변경](https://kimgoden.github.io/2021/11/15/kaggle-dataname/)
3. [불필요한 질문+NaN값 바꿔주기](https://kimgoden.github.io/2021/11/15/kaggle-dataedit/)


## 참고자료
- [.isin()에 관한 자료](https://www.geeksforgeeks.org/python-pandas-dataframe-isin/)
- [.isin()에 관한 자료2](https://cosmosproject.tistory.com/125)
- [.isin()에 관한 자료3](https://rfriend.tistory.com/460)
- [기존 데이터 프레임으로 부터 새오운 데이터 프레임 만들기](https://blog.naver.com/PostView.naver?blogId=frogsom1120&logNo=222110243527&categoryNo=11&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)
