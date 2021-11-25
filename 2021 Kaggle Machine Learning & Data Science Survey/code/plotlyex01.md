---
title: plotly 작성 연습 1
date: 2021-11-05 16:29:07
categories:
- 캐글
- 캐글 예제
tags:
- kaggle
- python
- plotly
- 시각화
---


# plotly 작성 연습 1
### - kaggle 에서 데이터셋을 불러와 히스토그램 그래프 작성
1. `pandas`, `numpy` 임포트 및 시각화를 위한 함수 세팅
```python
import numpy as np 
import pandas as pd 

import plotly.express as px #plotly 시각화 툴 임포트
from plotly.subplots import make_subplots  # 여러개의 그래프 표현 함수
import plotly.figure_factory as ff #특정한 그래프를 그리기 위한 함수
import plotly.graph_objs as go
```
2. 데이터프레임 셋팅
- `pd.read_csv()`함수는 `pandas` 라이브러리에서 제공하는 함수
외부 text파일, csv파일을 불러올 수 있게 해준다.
- `low_memory=False` 는 컬럼에 `Nan` 값이 여러 타입의 데이터가 섞여있을 경우 오류 발생 예방
```python
df = pd.read_csv('../input/kaggle-survey-2021/kaggle_survey_2021_responses.csv',low_memory=False)
questions = df.iloc[0, :].T
df = df.iloc[1:, :]
```

3. 직군별 남녀 비율을 표현한 히스토그램 작성

```python
df['Q2'] = df['Q2'].apply(lambda x : 'ETC' if x not in ['Man', 'Woman'] else x)
```
데이터프레임에 `Q2` 문항에서 Man, Woman이 아닌 값을 ETC에 반환해주는 코드
```python
x = df[df['Q2']!='ETC'][['Q5','Q2']]
```
히스토그램의 X값을 지정해주는 코드
```python
fig = px.histogram(x, y='Q5', color='Q2', title='Current position: Gender', histnorm='percent',
                  color_discrete_sequence=['#496595','#f36196'],
                  labels=dict(Q2="Gender"))
```
`fig()` 문
  + `px.histogram`문을 통해 히스토그램식으로 설정
  + 미리 설정한 x값을 x축에, y축은 `Q5`의 데이터항목으로 설정
  + 그래프의 제목(`title='Current position: Gender`)
  + 히스토그램에서 각 항목들이 전체 퍼센트 수치로 표현(`histnorm='percent'`)
  + 막대 색상의 값을 각각 다르게 지정(`color_discrete_sequence=['#496595','#f36196']`)
  + x축의 값이 된 Q2의 변수 두개를 표현(`labels=dict(Q2="Gender"))`)
```python
fig.update_traces(hovertemplate=None, marker=dict(line=dict(width=0)))
```
히스토그램의 마커에 대한 설정
  + 마커 설정을 위한 함수(`fig.update_traces()`)
  + 히스토그램 그래프에 마우스를 올릴 경우 나오는 `hover` 값을 설정해 주지 않음으로 기본적 값만 나오게 한다(`hovertemplate=None`)
  + 마커의 태두리 두깨를 설정해 준다(`marker=dict(line=dict(width=0)))`)
```python
fig.update_yaxes(showgrid=False, ticksuffix=' ', categoryorder='total ascending')
```
히스토그램 y축에 대한 설정
  + y축 설정을 위한 함수(`fig.update_yaxes()`)
  + 그래프 상 y축 좌표를 보이지 않게 한다(`showgrid=False`)
  + y축 변수명 옆 추가 설명란, (`ticksuffix= `) 코드상 여백이 들어가 있다. 
  + y축 정렬을 가장 많은 값 부터 내림차순으로 설정(`categoryorder='total ascending'`)
```python
fig.update_xaxes(visible=False)
```
히스토그램 x축에 대한 설정
  + 그래프 상 x축 좌표를 보이지 않게 한다.(`fig.update_xaxes(visible=False)`)
```python
fig.update_layout(height=1000, bargap=0.2, 
                  plot_bgcolor='#fff', paper_bgcolor='#fff',
                  margin=dict(b=0,r=20,l=20), 
                  title_font=dict(size=25, color='#333', family="Lato, sans-serif"),
                  font=dict(color='#8a8d93'), yaxis_title=" ", hovermode='y unified',
                  hoverlabel=dict(bgcolor="#333", font_size=13, font_family="Lato, sans-serif"),
                  legend=dict(orientation="h", yanchor="bottom", y=1, xanchor="center", x=0.5))
```
그래프 표현에 대한 설정
  + 위에 각 변수에 저장된 그래프들을 한번에 불러와 출력한다.<br>(`fig.update_layout`)</br>
  + `height=1000,` = 표 크기 / `bargap=0.2,` = 바 간격
  + x축과 y축 사이의 플로팅 영역의 배경색을 설정한다<br>(`plot_bgcolor`)</br>
  + 그래프 배경색을 설정해준다<br>(`paper_bgcolor`)</br>
  + 여백주기<br>(`margin=dict(b=0,r=20,l=20), `)</br> b=아래, r=오른쪽, l=왼쪽
  + 제목의 폰트 및, 크기 색상 지정<br>(`title_font=dict(size=25, color='#fff', family="Lato, sans-serif")</br>,`)
  + 그래프의 전반적인 폰트의 색상과 여백, 그리고 `hover` 타이틀이 될 변수를 설정해준다.<br>(`font=dict(color='#8a8d93'), yaxis_title=" ", hovermode='y unified',`)</br>
  + `hover` 의 폰트에 대한 설정 <br>(`hoverlabel=dict(bgcolor="#333", font_size=13, font_family="Lato, sans-serif"),`)</br>
  + 범례에 대한 설정 <br>(`legend=dict(orientation="h", yanchor="bottom", y=1, xanchor="right", x=0.5))`)</br>
  

4. 히스토그램 출력 결과
![](/images/plotly/plotly_histogram2.png)




### - 오류와 해결 방안
1.
![](/images/plotly/plotlyreader1.png)
- 원인 : 모듈을 import할 때 작업 폴더 내 모듈과 동일한 파일이 존재하기 때문입니다.
- 해결책 : `pd.read.csv()` 가 아닌 `pd.read_csv()` 로 변경 후 해결
- [참고](https://rfriend.tistory.com/250)


2.
![](/images/plotly/plotlyhistogramer1.png)
- 원인 : `Q2`의 데이터는 `Man`, `Woman` 만 있는 것이 아닌 다른 값들도 존제함.
그렇기 때문에 Q2 의 데이터에서 `ETC`의 값을 제외한 값만을 표현해야 했지만 `ETC가 제대로 설정되어 있지 않았음
- 해결책 : `lamdba` 함수에 if ~ else 문을 이용해 `Man`,`Woman`일 경우 Q2에 반환되지만, 아닐 경우 `ETC` 에 반환되도록 함
- [참고](https://www.delftstack.com/ko/howto/python/python-lambda-if/)



### - 참고한 자료
- [히스토그램 총 정리](https://ichi.pro/ko/plotly-expressleul-sayonghan-hiseutogeulaem-jeonche-gaideu-233660807404247)
- [Hover 정리](https://plotly.com/python/hover-text-and-formatting/)
- [layout 함수 총 정리](https://plotly.com/python/reference/layout/)

