---
title: pandas 연습2
date: 2021-11-15 16:53:16
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

```


```python
df = pd.read_csv('../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv',low_memory = False)
df = df[1:].reset_index(drop=True)
```


```python
df_asia = df[df['Q3'].isin(['Japan', 'China', 'South Korea'])]
df_usa = df[df['Q3'].isin(['United States of America'])]
```

## 새로운 데이터 프레임의 index를 새롭게 지정해보자


```python
df_asia.head()
```
![](/images/dataex05/dataex05_1/dataex05_1_a.png)
- 새로운 데이터 프레임의 인덱스 번호가 제각각이다. 이걸 1부터 차례대로 출력되게 바꿔보자


```python
df_asia.reset_index(list(range(0,2094,1)))
```
![](/images/dataex05/dataex05_1/dataex05_1_b.png)
- 처음 도전한 것은 `df_asia.reset_index(list(range(1,2094,1)))` 였다.
- 내가 `index`를 처음 만들때 처럼 1부터 2094까지 1씩 증가되는 인덱스를 만든다는 의미로 실행했는데 오류가 떴다.
- 정확한 의미는 모르겠지만 인덱스가 너무 많다는 뜻인거같다.


```python
df_asia.reset_index(drop=True)
```
![](/images/dataex05/dataex05_1/dataex05_1_c.png)

```python
df_usa.reset_index(drop=True)
```
![](/images/dataex05/dataex05_1/dataex05_1_d.png)
- `reset_index`는 내 생각보다 더 단순했다. 인덱스를 리셋해주고 기존걸 버리면 자동으로 새로운 인덱스를 추가해줬다.
- 간단하게 `resrt_index(drop=True)`를 해주는 것 만으로 기존 인덱스는 사라지고 0부터 끝까지 새로운 인덱스가 추가되었다.
- 인덱스 추가 클리어

## 참고자료
- [pandas-dataframe 인덱스 reset하기](https://bjy2.tistory.com/184)

