---
title: plotly 작성 연습 5
date: 2021-11-11 12:35:08
categories:
- 캐글
- 캐글 예제
tags:
- kaggle
- python
- plotly
- 시각화
- json
---

# plotly 작성 연습 5
### - kaggle 에서 데이터셋을 불러와 국가별 응답수를 막대그래프와 지도 형식으로 표현하기

```python

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px

df = pd.read_csv('../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv', low_memory=False)


```


```python
questions = df.iloc[0, :].T
questions
```


```python
df = df.iloc[1:, :].reset_index(drop = True)
df.head()
```


```python
countries = df.iloc[:, 3]
```
`countries`에 df의 4번째 열('Q3')을 반환해준다

```python
country_counts = (countries.value_counts().reset_index()).rename(columns = {'index':'Country','Q3': 'Count'})
```
`country_counts` 에 `countries`의 횟수를 넣어주고 `reset_index())` 한 후에 리네임으로 1행  `=index` 1열`=Country` ,<br> 2행 =`Q3`, 2열=`Count` 로 바꿔준다
```python
country_counts['Country']=country_counts['Country'].replace('Viet Nam', 'Vietnam')
```
`country_counts`의 `Country`에 있는 `Viet Nam` 을 `.replace`를 통해 `Vietnam`으로 바꿔주고 반환해준다. 
```python
from urllib.request import urlopen
```
urllib 패키지를 통해 인터넷 소스를 가져 올 수 있게 한다.
```python
import json
```
자바스크립트 기반으로 작성환 텍스트 데이터 포맷중 하나.
```python
with urlopen('https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json') as response:
    counties = json.load(response)
```
`urlopen`을 통해 해당 웹페이지의 데이터를 .json 형식으로 받아온다.
```python
info = pd.read_csv('https://raw.githubusercontent.com/plotly/datasets/master/2014_world_gdp_with_codes.csv')
```
`info`에 지도를 그리기 위한 csv값을 반환해준다.
```python
di = {}
```
딕셔너리 생성
```python
for _,row in info.iterrows():
    di[row['COUNTRY']]=row['CODE']
```
`info`에 반환받은 각 국가의 `CODE` 값과 `COUNTRY`를 새로운 `di`(딕셔너리)에 넣어준다.

```python
count=0
n=[]
c=[]
code=[]
for key, value in di.items():
    for _,row in country_counts.iterrows():
        if key in row['Country']:
            count=count+1
            n.append(key)
            c.append(row['Count'])
            code.append(value)
dfc=pd.DataFrame()
dfc['Country']=n
dfc['Count']=c
dfc['Code']=code

dfcn = pd.pivot_table(dfc, index=['Country'],values=['Count'],aggfunc='sum').reset_index()
dfc.drop('Count', axis=1, inplace=True)
dfcn = pd.merge(left=dfcn, right=dfc, on='Country')

dfcn =dfcn.sort_values(by="Count")
dfcn.drop_duplicates(inplace=True)

```
데이터 카테고리의 수를 카운트 해준다.
  - [참고](https://www.kaggle.com/j2hoon85/for-newbie-let-s-do-data-transformation)


### 1. 막대그래프 표현
```python
fig = px.bar(dfcn, x='Country', y='Count', height=600, color='Count', color_continuous_scale='pinkyl', template='plotly_white')
fig.update_xaxes(type='category')
fig.update_layout(title={
        'text': "Country wise user count",
        'x':0.5,
        'xanchor': 'center',
        'yanchor': 'top'})
fig.show()
```
 - 데이터셋은 `dfcn` x축은 국가명, y축은 응답수(카테고리의 수) 
 - 단순한 바가 아닌 응답수에 따라 색이 바뀌게(`color_contunuous_scale='pinkyl'`)

![](/images/plotly/barchar01.png)

### 2. 지도 표현
```python
fig = px.choropleth(dfcn, locations='Code', color='Count', color_continuous_scale='pinkyl',
                    hover_name="Country", template='plotly_white',
                    hover_data={
                        'Code':False
                    })
fig.update_layout(title={
        'text': "Demographic distribution of Users",
        'x':0.5,
        'xanchor': 'center',
        'yanchor': 'top'},dragmode=False)
fig.show()
```
 - 지도 표현을 위한 함수 `px.choropleth`(plotly.express 패키지 필요)
 - 지도상 정확 위치 표현을 위해 `locations=Code`를 기준으로 잡아준다.
 - 그 후 각 지역마다 응답수를 색상으로 표현

![](/images/plotly/mapchar01.png)

### 3. 참고자료

- [.iterrows 관련 자료1](https://fourier.dev/programming/2019/03/03/pandas_data_handling_2.html)
- [.iterrows 관련 자료2](https://dschloe.github.io/python/pandas/iterrows/)
- [DataFrame index reset자료](https://rfriend.tistory.com/456)
- [파이썬으로 json 데이터 읽고 쓰기](https://rfriend.tistory.com/474)
- [피봇 테이블 자료](https://wikidocs.net/46755)