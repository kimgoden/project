---
title: pandas 연습4
date: 2021-11-15 16:52:59
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

df = pd.read_csv('../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv',low_memory = False)
df = df[1:].reset_index(drop=True)

df_asia = df[df['Q3'].isin(['Japan', 'China', 'South Korea'])]
df_usa = df[df['Q3'].isin(['United States of America'])]
df_usa.reset_index(drop=True)
df_asia.reset_index(drop=True)
```


```python
df_usa.reset_index(drop=True)
```


```python
df_asia['Q3'].replace(['South Korea', 'Japan', 'China'],['KOR', 'JAP', 'CH'], inplace=True)
```


```python
df_usa['Q3'].replace('United States of America', 'USA', inplace=True)
```

## 필요없는 대답들 지우기+NULL값 다른 명칭으로 바꾸기
- 이 데이터는 kaggle 유저들의 답변들이 들어있다. 그래서 답변이 일정하지 않은 것들이 상당하다
- 예를들어 설명의 값만 보더라도


```python
df_asia['Q2'].unique()
```
![](/images/dataex05/dataex05_3/dataex05_3_a.png)
- 위에 처럼 `Man`, `Woman` 뿐 아니라 응답하지 않음, 나 자신을정의하지 않겠다. 남자여자 둘다 아니다 라는 답변이 있다.
- 우리에게 필요한 데이터는 남자와 여자 두 응답이라면 이 필요없는 데이터들은 따로 처리해야한다
- 먼저 떠오르는 것은 `Man`, `Woman`을 제외한 나머지 값들을 `NaN` 즉, null 값으로 만들어 주는 방법이다.
- 이와 같은 방법으로 우리에게 필요한 설문인 Q1,Q2,Q3,Q4,Q5,Q6,Q7,Q25,Q42 를 편집해보겠다.


```python
df_asia['Q2'].replace(['Prefer not to say', 'Prefer to self-describe','Nonbinary' ],[np.NaN,np.NaN,np.NaN], inplace=True)
```


```python
df_usa['Q2'].replace(['Prefer not to say', 'Prefer to self-describe','Nonbinary' ],[np.NaN,np.NaN,np.NaN], inplace=True)
```
![](/images/dataex05/dataex05_3/dataex05_3_b.png)
- 저번 index 때 학습했던 방법인 `replace`를 이용해 필요없는 값들을 nan 값으로 바꿔주었다
- 계속해서 다른 것들도 바꿔줘본다.
- 이번엔 반대로 null 값인 부분을 다른 값으로 채워보겠다.
- 예를들어 Q25의 경우 수익을 나타내지만 NaN 값이 상당부분 차지한다. 수익이란 데이터의 특이성을 생각하여 NaN 값이라 할지라도 시각화할 가치가 있기 때문에 다른 데이터를 채워넣어 주겠다.


```python
df_asia['Q25'].fillna('응답하지않음', inplace = True)
```


```python
df_asia['Q25'].head()
```
![](/images/dataex05/dataex05_3/dataex05_3_c.png)
- 위 코드처럼 `fillna` 를 이용하면 NaN 값을 다른 값으로 변경할 수 있다.
- 이떄 주의할 것은 위처럼 컬럼을 지정해주지 않으면 데이터 프레임의 모든 NaN값들이 다 바뀌게 된다.
- 그러니 반드시 특정 컬럼을 지정해주거나 하는 방식을 사용하는 것이 좋을거 같다.


## 참고자료
- [Youtube 나도코딩-파이썬 코딩 무료 강의 (활용편5)](https://youtu.be/PjhlUzp_cU0)