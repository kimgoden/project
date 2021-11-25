---
title: plotly_math1
date: 2021-11-08 16:46:57
categories:
- 파이썬
- 파이썬 문법
tags:
- Coalb
- python
---
 # 리스트 컴프리헨션 함수

1. 리스트 생성하기
```python
numbers = []
for n in range(1, 10+1):
    numbers.append(n)
```
위의 코드를 컴피리헨션으로 표기하면
```python
[x for x in range(10)]
```
- 컴프리헨션은 리스트 내부에 코드를 작성한다.

2. 조건 걸기
- 1부터 10까지 숫자중 짝수만 순차적으로 들어가 있는 리스트 생성
```python
even_numbers = []
for n in range(1, 10+1):
    if n % 2 == 0:
        even_numbers.append(n)
```

3. 컴프리헨션 for문 작성
``````