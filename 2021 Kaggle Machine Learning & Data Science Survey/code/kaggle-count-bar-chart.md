---
title: kaggle_count_bar_chart
date: 2021-11-18 21:56:13
categories:
- 캐글
- 캐글 시험 준비
tags:
- Coalb
- python
- pandas
- kaggle
---

## 4개국의 응답한 유저수를 하나의 막대 그래프로 표현하기

### 1. 모듈세팅
```python
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots
from plotly import graph_objects
import plotly.figure_factory as ff
```


```python
df_2021 = pd.read_csv('../input/my-data/kaggle_survey_2021_responses.csv')
df_2018 = pd.read_csv('../input/my-data/2018_kaggle_ds_and_ml_survey_responses_only.csv')
```

### 2, 데이터 전처리
```python
df_2021_asia = df_2021[df_2021['Q3'].isin(['Japan', 'China', 'South Korea'])].reset_index(drop=True)
df_2021_usa = df_2021[df_2021['Q3'].isin(['United States of America'])].reset_index(drop=True)
df_2018_asia = df_2018[df_2018['Q3'].isin(['Japan', 'China', 'South Korea'])].reset_index(drop=True)
df_2018_usa = df_2018[df_2018['Q3'].isin(['United States of America'])].reset_index(drop=True)
df_2021_data= df_2021[df_2021['Q3'].isin(['United States of America', 'Japan', 'China', 'South Korea'])]
```


```python
df_2021_data['Q3'].replace(['United States of America', 'South Korea', 'Japan', 'China'],['USA', 'KOR', 'JAP', 'CH'], inplace=True)
```


```python
df_2021_asia['Q3'].replace(['South Korea', 'Japan', 'China'],['KOR', 'JAP', 'CH'], inplace=True)
df_2021_usa['Q3'].replace('United States of America', 'USA', inplace=True)
df_2018_asia['Q3'].replace(['South Korea', 'Japan', 'China'],['KOR', 'JAP', 'CH'], inplace=True)
df_2018_usa['Q3'].replace('United States of America', 'USA', inplace=True)
```


```python
q3_df= df_2021_data.groupby(['Q3']).size().reset_index().rename(columns = {0:"Count"})

```

- 원하는 데이터만 남기고 지우는 작업을 했다. 제대로 되었다면 아래와 같은 결과가 나온다.

```python
q3_df.head()
```
![](/images/q3_df_head.png)

```python

def get_pnt(data, country):
    data_country = data[data['Q3'] == country].reset_index(drop = True)
    
    return data_country

usa_df = get_pnt(q3_df, "USA")
china_df = get_pnt(q3_df, "CH")
japan_df = get_pnt(q3_df, "JAP")
korea_df = get_pnt(q3_df, "KOR")
```

- `def` 함수를 통해 카운터 값을 제 정의 해주고 각 국가별 데이터 프레임에 반환해준다.
- 잘 되었다면 각 데이터 프레임마다 아래와 같은 결과가 나온다.

```python
korea_df
```
![](/images/korea_q3.png)

### 3. 그래프 시각화
```python


fig = go.Figure()
fig.add_trace(go.Bar(
     x= usa_df['Q3'],
     y=usa_df['Count'],
    text = usa_df['Count'],
     name='USA',
     
))
fig.add_trace(go.Bar(
     x= japan_df['Q3'],
     y=japan_df['Count'],
    text = japan_df['Count'],
     name='JAP',
     
))
fig.add_trace(go.Bar(
     x= china_df['Q3'],
     y=china_df['Count'],
    text = china_df['Count'],
     name='CH',
     
))
fig.add_trace(go.Bar(
     x= korea_df['Q3'],
     y=korea_df['Count'],
    text = korea_df['Count'],
     name='KOR',
     
))

fig.update_layout(barmode='group', xaxis_tickangle=-30,showlegend=True,
                 template = "plotly_white", title='2021 of Kaggle users')
fig.show()
```
![](/images/user_bar_1.png)