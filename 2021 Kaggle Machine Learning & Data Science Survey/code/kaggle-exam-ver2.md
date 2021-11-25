---
title: kaggle_exam_ver2
date: 2021-11-18 21:37:42
categories:
- 캐글
- 캐글 시험 준비
tags:
- Coalb
- python
- pandas
- kaggle
---

# 1. 모듈셋팅
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

# 2. 데이터 전처리

```python
df_2021_data= df_2021[df_2021['Q3'].isin(['United States of America', 'Japan', 'China', 'South Korea'])].reset_index(drop=True)
df_2021_asia = df_2021[df_2021['Q3'].isin(['Japan', 'China', 'South Korea'])].reset_index(drop=True)
df_2021_usa = df_2021[df_2021['Q3'].isin(['United States of America'])].reset_index(drop=True)
```


```python
df_2021_data['Q3'].replace(['United States of America', 'South Korea', 'Japan', 'China'],['USA', 'KOR', 'JAP', 'CH'], inplace=True)
df_2021_asia['Q3'].replace(['South Korea', 'Japan', 'China'],['KOR', 'JAP', 'CH'], inplace=True)
df_2021_usa['Q3'].replace('United States of America', 'USA', inplace=True)
```

# 3. 시각화작업
##  1. 각 국가 별 연령대 분포 파이차트 시각화

```python
q3_q1 = df_2021_data.groupby(['Q3', 'Q1']).size().reset_index().rename(columns = {0:"Count"})

def get_pnt(data, country):
    data_country = data[data['Q3'] == country].reset_index(drop = True)
    data_country['percentage'] = data_country["Count"] / data_country["Count"].sum()
    data_country['%'] = np.round(data_country['percentage'] * 100, 1)
    
    return data_country

usa_df = get_pnt(q3_q1, "USA")
china_df = get_pnt(q3_q1, "CH")
japan_df = get_pnt(q3_q1, "JAP")
korea_df = get_pnt(q3_q1, "KOR")
```
- `q3_q1` 이라는 데이터 프레임에 `Q3`,`Q1` 컬럼값을 넣어주고 그 수를 `Count` 컬럼에 넣어준다.
- `def ` 판다스 함수를 통해 빈도를 비율로 변환시켜주는 작업을 한다.
- `get_pnt`에 반환된 값들을 각 국가별 데이터프레임을 만들어주 넣어준다.

```python
fig = make_subplots(rows=2, cols=2, subplot_titles=("USA with Q1", "China with Q1", "Japan with Q1", "Korea with Q1"), column_widths = [2, 2],
                    specs=[[{'type':'domain'}, {'type':'domain'}],
                          [{'type':'domain'}, {'type':'domain'}]])

fig.add_trace(go.Pie(labels = usa_df['Q1'], 
                     values = usa_df['%'], 
                     ),  row = 1, col = 1)

fig.add_trace(go.Pie(labels = china_df['Q1'], 
                     values = china_df['%'], 
                      ), row = 1, col = 2)

fig.add_trace(go.Pie(labels = japan_df['Q1'], 
                     values = japan_df['%'], 
                    ),  row = 2, col = 1)

fig.add_trace(go.Pie(labels = korea_df['Q1'], 
                     values = korea_df['%'], 
                     ), row = 2, col = 2)
fig.update_layout(height = 1000,
                  showlegend=True,
                 template = "plotly_white")

fig.show()
```
![](/images/kaglle_my/q2.png)
- `make_subplots` 를 이용해 여러 그래프를 표현한다.
- 우선 그래프가 나올 범위를 `rows=2, cols=2` 로 설정했다.(이럴 경우 4개가 나온다)
- `specs=[[{'type':'domain'}, {'type':'domain'}],[{'type':'domain'}, {'type':'domain'}]]` 코드를 통해 각 pie 차트를 설정해준다.
- `labels`는 파이차트의 범주를, `values`는 할당 범위를 뜻한다.
- `row = 1, col = 1` 는 그래프의 위치를 뜻한다.


## 2. 각 국가별 프로그래밍 기간 분포 파이차트 시각화
```python
q3_q6 = df_2021_data.groupby(['Q3', 'Q6']).size().reset_index().rename(columns = {0:"Count"})

def get_pnt(data, country):
    data_country = data[data['Q3'] == country].reset_index(drop = True)
    data_country['percentage'] = data_country["Count"] / data_country["Count"].sum()
    data_country['%'] = np.round(data_country['percentage'] * 100, 1)
    
    return data_country

usa_df = get_pnt(q3_q6, "USA")
china_df = get_pnt(q3_q6, "CH")
japan_df = get_pnt(q3_q6, "JAP")
korea_df = get_pnt(q3_q6, "KOR")
```


```python
fig = make_subplots(rows=2, cols=2, subplot_titles=("USA with Q6", "China with Q6", "Japan with Q6", "Korea with Q6"), column_widths = [2, 2],
                    specs=[[{'type':'domain'}, {'type':'domain'}],
                          [{'type':'domain'}, {'type':'domain'}]])

fig.add_trace(go.Pie(labels = usa_df['Q6'], 
                     values = usa_df['%'], 
                     ),  row = 1, col = 1)

fig.add_trace(go.Pie(labels = china_df['Q6'], 
                     values = china_df['%'], 
                      ), row = 1, col = 2)

fig.add_trace(go.Pie(labels = japan_df['Q6'], 
                     values = japan_df['%'], 
                    ),  row = 2, col = 1)

fig.add_trace(go.Pie(labels = korea_df['Q6'], 
                     values = korea_df['%'], 
                     ), row = 2, col = 2)
fig.update_layout(height = 1000,
                  showlegend=True,
                 template = "plotly_white")

fig.show()
```
![](/images/kaglle_my/q3.png)

```python
q3_q5 = df_2021_data.groupby(['Q3', 'Q5']).size().reset_index().rename(columns = {0:"Count"})

def get_pnt(data, country):
    data_country = data[data['Q3'] == country].reset_index(drop = True)
    data_country['percentage'] = data_country["Count"] / data_country["Count"].sum()
    data_country['%'] = np.round(data_country['percentage'] * 100, 1)
    
    return data_country

usa_df = get_pnt(q3_q5, "USA")
china_df = get_pnt(q3_q5, "CH")
japan_df = get_pnt(q3_q5, "JAP")
korea_df = get_pnt(q3_q5, "KOR")
```


```python
fig = make_subplots(rows=2, cols=2, subplot_titles=("USA with Q5", "China with Q5", "Japan with Q5", "Korea with Q5"), column_widths = [2, 2],
                    specs=[[{'type':'domain'}, {'type':'domain'}],
                          [{'type':'domain'}, {'type':'domain'}]])

fig.add_trace(go.Pie(labels = usa_df['Q5'], 
                     values = usa_df['%'], 
                     ),  row = 1, col = 1)

fig.add_trace(go.Pie(labels = china_df['Q5'], 
                     values = china_df['%'], 
                      ), row = 1, col = 2)

fig.add_trace(go.Pie(labels = japan_df['Q5'], 
                     values = japan_df['%'], 
                    ),  row = 2, col = 1)

fig.add_trace(go.Pie(labels = korea_df['Q5'], 
                     values = korea_df['%'], 
                     ), row = 2, col = 2)
fig.update_layout(height = 1000,
                  showlegend=True,
                 template = "plotly_white")

fig.show()
```
![](/images/kaglle_my/q5.png)
