---
title: plotly 작성 연습 2
date: 2021-11-07 20:30:03
categories:
- 캐글
- 캐글 예제
tags:
- kaggle
- python
- plotly
- 시각화
---


# plotly 작성 연습 2
### - kaggle 에서 데이터셋을 불러와 Multi-Level Circle 그래프 그리기

1. 데이터프레임 셋팅은 [여기](https://kimgoden.github.io/2021/11/05/plotly/) 서 확인할 수 있다.
 * 단 `circlify` 그래프를 그리기 위해 모듈을 설치하는 방법의 경우 아래 코드를 입력
```python
!pip install circlify
```


```python
from pprint import pprint
import circlify
import matplotlib.pyplot as plt

```
 * kaggle notebook에서 pip install 하는 것을 알고 싶다면 [여기](https://kimgoden.github.io/categories/%EC%BA%90%EA%B8%80/%EC%BA%90%EA%B8%80-%EC%85%8B%ED%8C%85/) 서 확인 가능

2. '성별별 데이터 사이언티스트에게 입문 프로그래밍 언어 추천 응답에 대한' 데이터 셋
 * 해당 그래프의 작성법은 [여기](https://www.python-graph-gallery.com/circular-packing-several-levels-of-hierarchy) 를 바탕으로 작성했다.

```python
df1 = df[df['Q2']!='ETC']
df_q2_q8 = pd.crosstab(df1['Q8'], df1['Q2']).reset_index().sort_values(by='Man', ascending=False)
```
   1. `reset_index()`를 통해 인덱스를 처음으로 재 배열,<br>`sort_values(by='Man', ascending=False)`를 통해 Man을 기준으로 내림차순 정렬
```python
data = [{'id': 'World', 'datum': 50000, 'children' : [
              {'id' : "Python", 'datum' : 30000,  
                   'children' : [ 
                     {'id' : "Man", 'datum' : 16291},
                     {'id' : "Woman", 'datum' : 3570},
                   ]},
              {'id' : "R", 'datum' : 12000,  
                   'children' : [ 
                     {'id' : "Man", 'datum' : 1103},
                     {'id' : "Woman", 'datum' : 315},
                   ]},
              {'id' : "SQL", 'datum' : 8000,  
                   'children' : [ 
                     {'id' : "Man", 'datum' : 984},
                     {'id' : "Woman", 'datum' : 321},
                   ]},
    {'id' : "C++", 'datum' : 3000,  
                   'children' : [ 
                     {'id' : "Man", 'datum' : 347},
                     {'id' : "Woman", 'datum' : 85},
                   ]},
    {'id' : "C", 'datum' : 2000,  
                   'children' : [ 
                     {'id' : "Man", 'datum' : 328},
                     {'id' : "Woman", 'datum' : 99},
                   ]}
    ]}]
```
  2. 각 원들의 이름(`id: World`),  크기를 지정해주고(`datum: 50000`),<br> `childeren`은 원안에 작은 원이 들어감을 의미</br>
```python
data = [{'id': 'World', 'datum': 50000, 'children' : [ { 
```
   3. 각 원의 위치를 계산해준다.
      + `data`는 가장 큰 값에서 가장 작은 값으로 정렬되어있다.<br>`show_enclosure=False`는 그래프를 출력 여부를,</br>`target_enclosure=circlify.Circle(x=0, y=0, r=1))`는 그래프의 위치를 설정해준다.
```python
circles = circlify.circlify(
    data, # 필수
    show_enclosure=False, #옵션 
    target_enclosure=circlify.Circle(x=0, y=0, r=1)) #옵션
```
4. 여러 그래프를 출력하고, 그래프의 제목을 설정해준다.

```python
fig, ax = plt.subplots(figsize=(14,14))
ax.set_title('Top 5 Programming language are recommended to be a Data scientist')
```
  5. 그래프에서 x축과 y축 표현을 안보이게 해준다.
```python
ax.axis('off')
```
   6. 그래프의 각 원들이 서로 점에 마주닿게 설정한다.
 
```python

lim = max(
    max(
        abs(circle.x) + circle.r,
        abs(circle.y) + circle.r,
    )
    for circle in circles
)
plt.xlim(-lim, lim)
plt.ylim(-lim, lim)
```

   7. 가장 큰원의 출력값을 설정한다.
      - `if circle.level != 2:` 은 원의 출력 범위를 조절해준다.
      - `x, y, r = circle` 은 각각 원을 선언해준다
      - `ax.add_patch( plt.Circle((x, y), r, alpha=0.5, linewidth=2, color="lightblue"))`<br>은 r안에 x,y 원이 들어가며 원들의 태두리와 r의 배경색을 지정해준다.</br>
```python
for circle in circles:
    if circle.level != 2:
      continue
    x, y, r = circle
    ax.add_patch( plt.Circle((x, y), r, alpha=0.5, linewidth=2, color="lightblue"))
```
   8. 두번째 원의 설정이다.
      - 첫번째 원과 설정은 거의 동일하다.
      - 차이점은 색상과 원 중앙에 흰색 글씨를 넣는 코드가 추가된것이다.<br>`plt.annotate(label, (x,y ), ha='center', color="white")`</br>
```python
for circle in circles:
    if circle.level != 3:
      continue
    x, y, r = circle
    label = circle.ex["id"]
    ax.add_patch( plt.Circle((x, y), r, alpha=0.5, linewidth=2, color="#69b3a2"))
    plt.annotate(label, (x,y ), ha='center', color="white")
```
   9. 각 원이 뜻하는 범위의 텍스트 박스를 넣어준다.<br> `plt.annotate(label, (x,y ) ,va='center', ha='center', bbox=dict(facecolor='white', edgecolor='black', boxstyle='round', pad=.5))`</br>
```python
for circle in circles:
    if circle.level != 2:
      continue
    x, y, r = circle
    label = circle.ex["id"]
    plt.annotate(label, (x,y ) ,va='center', ha='center', bbox=dict(facecolor='white', edgecolor='black', boxstyle='round', pad=.5))
```

3. 그래프 출력 결과
![](/images/plotly/cirlce_03.png)
4. 참고자료
- [Circle Packing Chart with Multi-Level Hierarchy](https://www.python-graph-gallery.com/circular-packing-several-levels-of-hierarchy)
- [Basic Circle Packing Chart](https://www.python-graph-gallery.com/circular-packing-1-level-hierarchy)
