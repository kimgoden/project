---
title: plotly 작성 연습 6
date: 2021-11-12 16:47:18
categories:
- 캐글
- 캐글 예제
tags:
- kaggle
- python
- plotly
- 시각화
---

# plotly 작성 연습 6
### - kaggle 에서 데이터셋을 불러와 국가별 트리맵 형식으로 표현하기


### 보충 필요
```python
fig = px.treemap(dfcn, path=['Country','Count'], color='Count')

fig.update_layout(title="<b>Countries in the 2021 Survey<b>",
                  titlefont={'size': 24, 'family': "San Serif"},
                  height=500, width=700,
                  template='simple_white',
                  paper_bgcolor='#F5F5F5',
                  #plot_bgcolor='#F5F5F5',
                  autosize=False,
                  margin=dict(l=50,r=50,b=50, t=250,
                             ),
                 )
fig.update_layout(margin = dict(t=50, l=50, r=50, b=100))

annotations = []
annotations.append(dict(xref='paper', yref='paper',
                        x=-0.01, y=-0.1,
                        showarrow=False))
annotations.append(dict(xref='paper', yref='paper',
                        x=-0.01, y=-0.2,
                        showarrow=False))

fig.update_layout(annotations=annotations)
fig.show()
```


### 시각화 결과
![](/images/plotly/treemap01.png)