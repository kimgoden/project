---
title: plotly 작성 연습 3
date: 2021-11-09 10:45:20
categories:
- 캐글
- 캐글 예제
tags:
- kaggle
- python
- plotly
- 시각화
---

# plotly 작성 연습 3
### - kaggle 에서 데이터셋을 불러와 top10 국가 그래프 두개 그리기

1. 데이터 정리
```python
# converting : Man, Woman을 제외한 값은 ETC
df['Q2'] = df['Q2'].apply(lambda x : 'ETC' if x not in ['Man', 'Woman'] else x)
```
필요한 `Man`,`Woman` 값을 제외한 나머지는 `ETC`에 저장

```python
# replace : 국가명을 하나로 일치
df['Q3'] = df['Q3'].replace(['United States of America','United Kingdom of Great Britain and Northern Ireland'], ['USA','UK & NI'])
```
데이터프레임에 저장된 국가명 중, 같은 나라지만 다른 표기로 된 것을 하나로 통일

2. 시각화 작업
```python
df = df[df['Q3']!='ETC']

df1 = pd.crosstab(df['Q2'], df['Q3'], margins=True).T.sort_values(by='All', ascending=False)[:11].iloc[1: , :].reset_index()
```
`crosstab` 을 통해 `Q2`, `Q3` 를 교차분석 하고, 정렬을 해준다
```python
fig = make_subplots(rows=1, cols=2, column_widths=[0.4,0.6], shared_yaxes=True, horizontal_spacing=0)
fig.append_trace(go.Bar(x=df_c['Woman'], y=df_c.Q3, orientation='h', showlegend=True, 
                        text=df_c['Woman'], name='Woman', marker_color='#6D83AA'), 1, 1)
fig.append_trace(go.Bar(x=df_c['Man'], y=df_c.Q3, orientation='h', showlegend=True, 
                        text=df_c['Man'], name='Man', marker_color='#334668'), 1, 2)
```
두개의 그래프의 위치, 들어갈 값, 변수를 정해주고 스타일을 꾸며준다.
```python
fig.update_xaxes(showgrid=False, zeroline=False)
fig.update_yaxes(ticksuffix='  ')
```
그래프상 좌표를 보이지 않게 한다.
```python
fig.update_traces(hovertemplate=None, marker=dict(line=dict(width=0)),
                  textposition='auto')
```
그래프에 마우스를 가져갔을 때 나오는 호버값을 정해준다.<br>
따로 정해지지 않았음으로 기본적 정보만 나온다.
```python
fig.update_layout(height=500, barmode='overlay', 
                  title="<span style='font-size:50px; font-family:Times New Roman'>Top 10 Country</span>",
                  margin=dict(t=110, b=40, l=120, r=40),               
                  plot_bgcolor='#333', paper_bgcolor='#333',
                  title_font=dict(size=30, color='#8a8d93', family="Lato, sans-serif"),
                  font=dict(size=13, color='#8a8d93'),
                  legend=dict(title="", orientation="v", yanchor="bottom", xanchor="center", x=0.83, y=1.05,
                              bordercolor="#fff", borderwidth=0.5, font_size=13))
```
그래프의 크기, 제목, 여백, 배경색, 글씨체, 등등 스타일을 정해준다
```python
fig.show()
```
그래프를 출력한다.

3. 그래프 출력 결과
![](/images/plotly/top10country.png)


4. 참고자료
참고: https://rfriend.tistory.com/280 [R, Python 분석과 프로그래밍의 친구 (by R Friend)])