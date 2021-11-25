---
title: kaggle_exam_ver1
date: 2021-11-17 17:27:20
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
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots
from plotly import graph_objects


```


```python
df_2021 = pd.read_csv('../input/my-data/kaggle_survey_2021_responses.csv')
df_2018 = pd.read_csv('../input/my-data/2018_kaggle_ds_and_ml_survey_responses_only.csv')
```

# plotly를 이용해 그래프 막대그래프 그리기
- plotly를 이용하기 전, 먼저 `pandas`와 필요한 시각화툴(`plotly`)를 임포트 해준다 
- 필요한 데이터를 `pandas`를 이용해 불러와서 데이터 프레임에 저장한다
- 예를들어 `kaggle_survey_2021_responses.csv` 이란 파일을 `pandas`에 내장된 `pd.read_csv` 명령어를 이용해 `df_2021`이란 데이터 프레임에 넣었다.


```python
df_2021_data= df_2021[df_2021['Q3'].isin(['United States of America', 'Japan', 'China', 'South Korea'])]
df_2021_asia = df_2021[df_2021['Q3'].isin(['Japan', 'China', 'South Korea'])]
df_2021_usa = df_2021[df_2021['Q3'].isin(['United States of America'])]
df_2021_data.reset_index(drop=True)
```

- 위에서 만든 데이터 프레임을 한번더 쪼개서 3개의 데이터 프레임을 추가로 만들었다
  - 각각 `df_2021_data` = 한중일미 / `df_2021_asia` = 한중일 / `df_2021_usa` = 미국
- 이처럼 기존 데이터 프레임에서 원하는 컬럼(`Q3`) 을 지정하고 원하는 값이 있는 행만 가져와 새로운 데이터 프레임을 만들었다.
  - 예시 `df_2021_data= df_2021[df_2021['Q3'].isin(['United States of America', 'Japan', 'China', 'South Korea'])]`
  - 새로운 데이터 프레임 = 기존 데이터 프레임 [기존데이터 프레임 [기존데이터 프레임 ['컬럼명'].<strong>isin</strong> (['원하는 값1', '원하는 값2', 등등..])


```python
df_2021_usa.reset_index(drop=True)
```


```python
df_2021_asia.reset_index(drop=True)

```

- 데이터 프레임이 잘 만들어졌나 확인하고, `reset_index(drop=True)` 을 통해 인덱스 번호를 재정렬해준다.


```python
df_2021_data['Q3'].replace(['United States of America', 'South Korea', 'Japan', 'China'],['USA', 'KOR', 'JAP', 'CH'], inplace=True)
```


```python
df_2021_data['Q2'].replace(['Prefer not to say', 'Prefer to self-describe','Nonbinary' ],[np.NaN,np.NaN,np.NaN], inplace=True)
```


```python
df_2021_asia['Q3'].replace(['South Korea', 'Japan', 'China'],['KOR', 'JAP', 'CH'], inplace=True)
```


```python
df_2021_usa['Q3'].replace('United States of America', 'USA', inplace=True)
```

- 각 데이터 프레임에서 길고 복잡한 값들의 이름이나 필요없는 값 자체를 `NaN` 값으로 바꿔준다
- 이때 `.replace()` 를 사용하면 좋다.
  - 예시 : 데이터프레임['컬럼명'].<strong>replace</strong>(['기존 값1', '기존 값2', 등등,,] , ['변경할 값1', '변경할 값2', 등등..])
  - `.replace()` 안에는 내가 지정한 컬럼에서 바꾸고 싶은 만큼의 값을 넣고, 뒤에 변경할 값도 넣은 값의 수 만큼 넣어준다.
  - 만약 `NaN` 으로 바꾸고 싶다면 'NaN' 이라고 넣는것이 아닌 `np.NaN` 이라고 넣어주면 된다.
  - 변경을 한 후 실제 데이터 프레임에 반영하기 위해선 () 안에 `, inplace=True`  넣어줘야 한다.

# 한중일 vs 미국 연령대 분포 비교


```python
df_2021_asia_q1 = df_2021_asia['Q1'].value_counts(ascending=True).reset_index()
df_2021_asia_q1.head()
```

## 1. 빈도에 따른 그래프 그리기 
 - `df_2021_asia_q1` 라는 데이터 프레임을 하나 만든다.
 - 새로운 데이터 프레임 = 기존 데이터 프레임 ['컬럼명'].`value_counts`(컬럼들의 값들 중 중복값을 제외한 값들을 카운팅 해준다)
 - `(ascending=True)`(작은값 부터 내림차순으로 커지게 정렬한다)
 - `.reset_index()` 인덱스를 재 배열해준다.(꼭 해야한다)


```python
df_2021_asia_q1 = df_2021_asia['Q1'].value_counts(ascending=True).reset_index()
# 이 위 코드는 따로 적었다면 또 안적어도 된다
fig = px.bar(df_2021_asia_q1,
             x= 'index',
             y=df_2021_asia_q1['Q1'],
             color='Q1',
             title='Aisa of Age Distributions on Kaggle'
            )
      
fig.show()
```
![](/images/asia_age_ver1.png)

```python
df_2021_usa_q1 = df_2021_usa['Q1'].value_counts(ascending=True).reset_index()
# 이 위 코드는 따로 적었다면 또 안적어도 된다
fig = px.bar(df_2021_usa_q1,
             x= "index",
             y=df_2021_usa_q1['Q1'],
             color='Q1',
             title='USA of Age Distributions on Kaggle'
            )

      
fig.show()
```
![](/images/usa_age_ver1.png)
- 코드 해석
 - `fig = px.bar` = `plotly.express`를 임포트 해야 사용 가능한 그래프 그리기 도구다.
 - `df_2021_usa_q1,` = 내가 그래프를 그릴때 사용할 데이터 프레임이다
 - `x= "index",` = x축에 들어갈 값이 `df_2021_usa_q1` 데이터 프레임에 들어있는 `index`라는 컬럼이라는 뜻이다.
 - `y=df_2021_usa_q1['Q1'],` = y축에 들어갈 값이 `df_2021_usa_q1` 데이터 프레임에 들어있는 `Q1` 이라는 컬럼이라는 뜻을 다르게 쓴거다.
 - `color='Q1',` = 막대의 색을 구분하는 기준을 `Q1`에 있는 데이터들을 기준으로 나누겠다는 뜻이다.
 - `title='USA of Age Distributions on Kaggle'` = 내 그래프 제목이다
 - `fig.show()` = 그래프를 보여주겠다는 뜻이다


```python
fig = go.Figure()
fig.add_trace(go.Bar(
     x= df_2021_asia_q1['index'],
     y=df_2021_asia_q1['Q1'],
     name='Asia of Age',
     marker_color='indianred'
))
fig.add_trace(go.Bar(
     x= df_2021_usa_q1['index'],
     y=df_2021_usa_q1['Q1'],
     name='USA of Age',
     marker_color='lightsalmon'
))

fig.update_layout(barmode='group', xaxis_tickangle=-45)
fig.show()
```
![](/images/asia_usa_age_ver1.png)
- 코드해석
- 먼저 위처럼 막대를 두개 겹칠라면 위에서 
`import plotly.graph_objects as go
from plotly.subplots import make_subplots
from plotly import graph_objects` 를 입력해주어야한다.

 - `fig = go.Figure()` = 그래프를 겹쳐서 그리고 싶을때. 위에 `px` 대신 사용하는 그래프 그리는 도구다.
 - `fig.add_trace(go.Bar` = `fig.add_trace` 는 그냥 쓰면된다. 중요한건 `go.Bar` 다 그래프 형식을 설정해준다. 첫 글자가 무조건 대문자다.
 - `x= df_2021_asia_q1['index'],` = x축 값이다.
 - `y=df_2021_asia_q1['Q1'],` = y축 값이다.
 - `name='Asia of Age',` = 막대의 이름이다 옆에 조그맣게 뜬다.
 - `marker_color='indianred' ` =막대의 색깔이다.
 - 더 추가로 설정을 해줘도 되지만 한 그래프를 다 설정하고 나면 반드시 )) 두개를 닫아줘야한다.
 - `fig.update_layout(barmode='group', xaxis_tickangle=-45)` = 그래프 디자인에 대한 설정이다 설정해주면 그래프의 간격들을 설정해주었다.
 
 

## 2. 비율에 따른 그래프 그리기
 - 새로운 데이터 프레임 = `pd.DataFrame`(기존데이터 프레임['컬럼명']`.value_counts())` = 기존 프레임의 원하는 컬럼의 값을 카운트해서 새 프레임에 넣어준다.
 - 새로운데이터 프레임`.column` = `["원하는 컬럼명"]` = 위에서 카운트한 값을 컬럼명 `Q1` 에 넣는다.
 - 새로운 데이터 프레임["빈도를 의미하는 컬럼명"] = `np.round(새로운 데이터 프레임`.`위에서 설정한 컬럼명/sum(새로운 데이터 프레임.위에서 설정한 컬럼명),2)`
 


```python
df_2021_usa_q1 = pd.DataFrame(df_2021_usa['Q1'].value_counts())
df_2021_usa_q1.column =["Q1"]
df_2021_usa_q1["ratio"] = np.round(df_2021_usa_q1.Q1/sum(df_2021_usa_q1.Q1),2)
df_2021_usa_q1
```


```python
df1=df_2021_usa_q1.reset_index()
```

- 비율도 구하고나면 `.rest_index()` 한번 해준다.


```python
df_2021_usa_q1 = pd.DataFrame(df_2021_usa['Q1'].value_counts())
df_2021_usa_q1.column =["Q1"]
df_2021_usa_q1["ratio"] = np.round(df_2021_usa_q1.Q1/sum(df_2021_usa_q1.Q1),2)
df1=df_2021_usa_q1.reset_index()

fig = px.bar(df1,
             x= df1['index'],
             y=df1['ratio'],
             color='ratio',
             title='USA of Age Distributions on Kaggle'
            )

      
fig.show()
```
![](/images/usa_age_ver2.png)

```python
df_2021_asia_q1 = pd.DataFrame(df_2021_asia['Q1'].value_counts())
df_2021_asia_q1.column =["Q1"]
df_2021_asia_q1["ratio"] = np.round(df_2021_asia_q1.Q1/sum(df_2021_asia_q1.Q1),2)
df_2021_asia_q1['%'] = np.round(df_2021_asia_q1['ratio'] * 100, 1)
df2=df_2021_asia_q1.reset_index()

fig = px.bar(df2,
             x= df2['index'],
             y=df2['%'],
             color='%',
             text = df2['%'].astype(str)+'%',
             title='asia of Age Distributions on Kaggle'
            )

      
fig.show()
```
![](/images/asia_age_ver2.png)

```python
df_2021_usa_q1 = pd.DataFrame(df_2021_usa['Q1'].value_counts())
df_2021_usa_q1.column =["Q1"]
df_2021_usa_q1["ratio"] = np.round(df_2021_usa_q1.Q1/sum(df_2021_usa_q1.Q1),2)
df_2021_usa_q1['%'] = np.round(df_2021_usa_q1['ratio'] * 100, 1)
df1=df_2021_usa_q1.reset_index()

fig = px.bar(df1,
             x= df1['index'],
             y=df1['%'],
             color='%',
             text = df1['%'].astype(str)+'%',
             title='USA of Age Distributions on Kaggle'
            )

      
fig.show()
```
![](/images/usa_age_ver3.png)
- 위처럼 막대에 글자를 띄우고 싶다면 `text = 데이터프레임명['띄우고싶은 값'].astype(str)+ '추가로 띄우고 싶은 문구',`


```python
fig = go.Figure()
fig.add_trace(go.Bar(
     x= df2['index'],
     y=df2['%'],
    text = df2['%'].astype(str)+'%',
     name='Asia of Age',
     marker_color='indianred'
))
fig.add_trace(go.Bar(
     x= df1['index'],
     y=df2['%'],
    text = df1['%'].astype(str)+'%',
     name='USA of Age',
     marker_color='lightsalmon'
))

fig.update_layout(barmode='group', xaxis_tickangle=-45)
fig.show()
```
![](/images/asia_usa_age_ver2.png)
# 한중일 vs 미국 프로그래밍 기간 비교


```python
df_2021_asia_q6 = df_2021_asia['Q6'].value_counts(ascending=True).reset_index()
```


```python
df_2021_asia_q6.head()
```


```python
df_2021_usa_q6 = df_2021_usa['Q6'].value_counts(ascending=True).reset_index()
```


```python
df_2021_asia_q6 = pd.DataFrame(df_2021_asia['Q6'].value_counts())
df_2021_asia_q6.column =["Q6"]
df_2021_asia_q6["ratio"] = np.round(df_2021_asia_q6.Q6/sum(df_2021_asia_q6.Q6),2)
df_2021_asia_q6['%'] = np.round(df_2021_asia_q6['ratio'] * 100, 1)
df2_q6=df_2021_asia_q6.reset_index()
```


```python
df1_q6.head()
```


```python
df_2021_usa_q6 = pd.DataFrame(df_2021_usa['Q6'].value_counts())
df_2021_usa_q6.column =["Q6"]
df_2021_usa_q6["ratio"] = np.round(df_2021_usa_q6.Q6/sum(df_2021_usa_q6.Q6),2)
df_2021_usa_q6['%'] = np.round(df_2021_usa_q6['ratio'] * 100, 1)
df1_q6=df_2021_usa_q6.reset_index()
```


```python
df2_q6.head()
```


```python
fig = px.bar(df1_q6,
             x= df1_q6['index'],
             y=df1_q6['%'],
             color='%',
             text = df1_q6['%'].astype(str)+'%',
             title='The period that USA programmed.'
            )
fig.show()
```
![](/images/usa_program_ver1.png)

```python
fig = px.bar(df2_q6,
             x= df2_q6['index'],
             y=df2_q6['%'],
             color='%',
             text = df2_q6['%'].astype(str)+'%',
             title='The period that Asia programmed.'
            )
fig.show()
```
![](/images/asia_program_ver1.png)
# 한중일 vs 미국 직업군 비율 비교



```python
df_2021_asia_q5 = df_2021_asia['Q5'].value_counts(ascending=True).reset_index()
df_2021_asia_q5.head(15)
```


```python
df_2021_usa_q5 = df_2021_usa['Q5'].value_counts(ascending=True).reset_index()
df_2021_usa_q5.head(15)
```


```python
df_2021_asia_q5 = pd.DataFrame(df_2021_asia['Q5'].value_counts())
df_2021_asia_q5.column =["Q5"]
df_2021_asia_q5["ratio"] = np.round(df_2021_asia_q5.Q5/sum(df_2021_asia_q5.Q5),2)
df_2021_asia_q5['%'] = np.round(df_2021_asia_q5['ratio'] * 100, 1)
df2_q5=df_2021_asia_q5.reset_index()

fig = px.bar(df2_q5,
             x= df2_q5['index'],
             y=df2_q5['%'],
             color='%',
             text = df2_q5['%'].astype(str)+'%',
             title='Classification of jobs in the Asia'
            )
fig.show()
```
![](/images/asia_jobs_ver1.png)

```python
df_2021_usa_q5 = pd.DataFrame(df_2021_usa['Q5'].value_counts())
df_2021_usa_q5.column =["Q5"]
df_2021_usa_q5["ratio"] = np.round(df_2021_usa_q5.Q5/sum(df_2021_usa_q5.Q5),2)
df_2021_usa_q5['%'] = np.round(df_2021_usa_q5['ratio'] * 100, 1)
df1_q5=df_2021_usa_q5.reset_index()

fig = px.bar(df1_q5,
             x= df1_q5['index'],
             y=df1_q5['%'],
             color='%',
             text = df1_q5['%'].astype(str)+'%',
             title='Classification of jobs in the USA'
            )
fig.show()
```
![](/images/usa_jobs_ver1.png)