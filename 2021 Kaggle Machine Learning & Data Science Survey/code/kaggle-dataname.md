---
title: pandas 연습3
date: 2021-11-15 16:50:13
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
```


```python
df = pd.read_csv('../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv',low_memory = False)
df = df[1:].reset_index(drop=True)
```


```python
df_asia = df[df['Q3'].isin(['Japan', 'China', 'South Korea'])]
df_usa = df[df['Q3'].isin(['United States of America'])]
df_usa.reset_index(drop=True)
df_asia.reset_index(drop=True)
```


```python
df_asia.reset_index(drop=True)
```

## 데이터 프레임에서 데이터명을 바꿔보자
- `South Korea` 는 정식 명칭이 아니고, `United States of America` 같은 경우 너무 길다.
- 해당 명칭들을 한눈에 들어오기 좋게 약어로 바꿔보겠다.


```python
df_asia['Q3'].replace(['South Korea', 'Japan', 'China'],['KOR', 'JAP', 'CH'], inplace=True)
```


```python
df_asia.head()
```
![](/images/dataex05/dataex05_2/dataex05_2_a.png)

```python
df_usa['Q3'].replace('United States of America', 'USA', inplace=True)
df_usa.head()
```
![](/images/dataex05/dataex05_2/dataex05_2_b.png)
- `replace` 를 이용해 국가명을 보기 좋게 바꿀 수 있었다.
- 처음엔 `,inplace=True`를 넣지 않았더니 데이터 프레임 원본은 바뀌지 않았다.
- 까먹지 말자 원본에 반영하기 위해선 `, inplace=True` 가 필수라는 것을!

## 참고자료
- [Pandas DataFrame에서 열 값 바꾸기](https://www.delftstack.com/ko/howto/python-pandas/pandas-replace-values-in-column/)
