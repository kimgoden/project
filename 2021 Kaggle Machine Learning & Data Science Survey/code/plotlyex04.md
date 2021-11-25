---
title: plotly 작성 연습 4
date: 2021-11-09 11:06:30
categories:
- 캐글
- 캐글 예제
tags:
- kaggle
- python
- plotly
- 시각화
---


# plotly 작성 연습 4
### - kaggle 에서 데이터셋을 불러와 국가 별 응답수와 특정 국가 % 구하기

1. 데이터 세팅
```python
country = 'South Korea'
```
`country =` 를 사용해 본인이 원하는 국가슬 설정한다.<br>
이때 원하는 컬럼명과 반드시 일치해야 한다.
```python
if country not in df[df.columns[3]].unique():
    raise ValueError(f'{country} not found in the list')
```
국가명을 가져오는 과정에서 오류가 뜰 경우 `f'{country} not found in the list` 가 뜨고 일단 실행되게 한다.
```python
df['country_agg'] = np.where(df[df.columns[3]]==country,country,'Others')
```
df의 국가명에 조건을 걸어서 배열에 반환한다.

2. 시각화 작업
```python
fig = px.pie(df, df.columns[3], 
       title=f"{len(df[df[df.columns[3]]==country])*100/len(df):.2f}% of all survey respondents are from {country}", 
       hole=0.6)
```
그래프 상에 국가명과 차지하는 %가 몇인지 뜨게 해준다.
```python
fig.update_traces(textposition='inside', textinfo='percent+label')
fig.update_layout(uniformtext_minsize=10, paper_bgcolor='#333',font=dict(size=13, color='#8a8d93'), uniformtext_mode='hide')
fig.show()
```
그래프의 전체적인 스타일을 설정해준다.

4. 그래프 결과
![](/images/plotly/ALLcounty.png)

5. 참고자료
- [유일한 값(unique value)](https://rfriend.tistory.com/267)
- [에러와 예외](https://docs.python.org/ko/3/tutorial/errors.html)
- [pd.where과 np.where의 차이](https://yganalyst.github.io/data_handling/memo_3/)