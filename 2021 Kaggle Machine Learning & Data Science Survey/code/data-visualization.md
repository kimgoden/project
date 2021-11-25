---
title: 파이썬 시각화 1
date: 2021-11-03 12:58:15
categories:
- 파이썬
- 데이터 시각화
tags:
- python
- visualization
- colab
---

# 파이썬 시각화
 - .legend()(범례)는 그래프에 데이터의 종류를 표시하기위한 메서드다.
 - 그래프 영역에 범례를 나타내기 위해선 먼저 plot() 함수에 label 문자열을 지정한다.

<!-- more -->
```python
# 파이썬 시각화를 위한 모듈세팅
import matplotlib.pyplot as plt

# 데이터 준비
dates = [
         '2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04', '2021-01-05',
         '2021-01-06', '2021-01-07', '2021-01-08', '2021-01-09', '2021-01-10'
]
min_temperature = [20.7, 17.9, 18.8, 14.6, 15.8, 15.8, 15.8, 17.4, 21.8, 20.0]
max_temperature = [34.7, 28.9, 31.8, 25.6, 28.8, 21.8, 22.8, 28.4, 30.8, 32.0]

# 하나의 ax 만을 가지는 하나의 figure 생성
# figure 는 그래프를 그릴 공간을 의미하고 ax(axes)는 공간에 내가 사용할 부분을 의미한다.
fig, ax = plt.subplots()

# 그래프 그리기
ax.plot(dates, min_temperature, label = "Min Temp")
ax.plot(dates, max_temperature, label = "Max Temp")
ax.legend()
plt.show()
```


    
![png](/images/visual/output_1_0.png)
    



```python
import matplotlib.pyplot as plt

dates = [
    '2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04', '2021-01-05',
    '2021-01-06', '2021-01-07', '2021-01-08', '2021-01-09', '2021-01-10'
]
min_temperature = [20.7, 17.9, 18.8, 14.6, 15.8, 15.8, 15.8, 17.4, 21.8, 20.0]
max_temperature = [34.7, 28.9, 31.8, 25.6, 28.8, 21.8, 22.8, 28.4, 30.8, 32.0]

fig, axes = plt.subplots(nrows=1, ncols=1, figsize=(10,6))
# 위의 코드와 진행은 동일하지만, figure의 사이즈를 지정해준다.
axes.plot(dates, min_temperature, label = 'Min Temperature')
axes.plot(dates, max_temperature, label = 'Max Temperature')
axes.legend()
plt.show()
```


    
![png](/images/visual/output_2_0.png)
    



```python
print(fig)
print(axes)
```

    Figure(720x432)
    AxesSubplot(0.125,0.125;0.775x0.755)
    

# Pyplot API + 객체지향 API
- Pyplot API : 이전절에 소개한 Matlab과 같이 커맨드 방식. matplotlib.pyplot 모듈에 함수로 정의되어 있음.
- 객체지향 API : matplotlib이 구현된 객체지향라이브러리를 직접 활용하는 방식.
- Pyplot API는 결국 객체지향 API로 편의함수를 구현한 것 뿐이기에 세밀한 제어를 위해서 객체지향 API를 사용해야한다.


```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0,1,50)
y1 = np.cos(4*np.pi*x)
y2 = np.cos(4*np.pi*x)*np.exp(-2*x)

fig,ax = plt.subplots(figsize=(10, 6))  # plt.subplots() 편의 함수는 Figure 객체를 생성하고 Figure.subplots()를 호출하여 리턴
ax.plot(x,y1,'r-*',lw=1)
ax.plot(x,y2,'b--',lw=1)
```




    [<matplotlib.lines.Line2D at 0x7f83450f0690>]




    
![png](/images/visual/output_5_1.png)
    


 ## 주가 데이터 패키지를 받아와 시각화 표현 예시
  - Yahoo Finance 사이트에서 주가데이터를 받아와본다.
    1. 먼저 `!pip install yfinance` 를 통해 패키지 설치 
    2. `import fix_yahoo_finance as yf` Yahoo 데이터를 yf에 임포트 해준다.
    3. `import yfinance as yf` yf에 임포트해준다.


```python
!pip install yfinance
import fix_yahoo_finance as yf
import yfinance as yf
import matplotlib.pyplot as plt

data = yf.download('AAPL', '2019-08-01', '2020-08-01')
ts = data['Open']

fig, ax = plt.subplots(figsize=(10, 6)) # 직접 Figure 객체 생성
# ax= fig.subplots() # 직접 axes를 생성
ax.plot(ts) # 생성된 axes 에 대한 plot() 멤버 직접 호출 
ax.set_title('Stock Market fluctuation of AAPL')
ax.legend(labels=['Price'], loc='best')
ax.set_xlabel('Date')
ax.set_ylabel('Stock Market Open Price')
plt.show()
```

    Requirement already satisfied: yfinance in /usr/local/lib/python3.7/dist-packages (0.1.64)
    Requirement already satisfied: numpy>=1.15 in /usr/local/lib/python3.7/dist-packages (from yfinance) (1.19.5)
    Requirement already satisfied: multitasking>=0.0.7 in /usr/local/lib/python3.7/dist-packages (from yfinance) (0.0.9)
    Requirement already satisfied: lxml>=4.5.1 in /usr/local/lib/python3.7/dist-packages (from yfinance) (4.6.4)
    Requirement already satisfied: pandas>=0.24 in /usr/local/lib/python3.7/dist-packages (from yfinance) (1.1.5)
    Requirement already satisfied: requests>=2.20 in /usr/local/lib/python3.7/dist-packages (from yfinance) (2.23.0)
    Requirement already satisfied: python-dateutil>=2.7.3 in /usr/local/lib/python3.7/dist-packages (from pandas>=0.24->yfinance) (2.8.2)
    Requirement already satisfied: pytz>=2017.2 in /usr/local/lib/python3.7/dist-packages (from pandas>=0.24->yfinance) (2018.9)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.7/dist-packages (from python-dateutil>=2.7.3->pandas>=0.24->yfinance) (1.15.0)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /usr/local/lib/python3.7/dist-packages (from requests>=2.20->yfinance) (1.24.3)
    Requirement already satisfied: chardet<4,>=3.0.2 in /usr/local/lib/python3.7/dist-packages (from requests>=2.20->yfinance) (3.0.4)
    Requirement already satisfied: idna<3,>=2.5 in /usr/local/lib/python3.7/dist-packages (from requests>=2.20->yfinance) (2.10)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.7/dist-packages (from requests>=2.20->yfinance) (2021.5.30)
    [*********************100%***********************]  1 of 1 completed
    


    
![png](/images/visual/output_7_1.png)
    


# 막대 그래프 작성법
 - `import calendar` 을 통해 캘린더 데이터를 받아온다.
 - `for`문을 통해 조건을 반복해 1~12월까지 반복해준다.


```python
import matplotlib.pyplot as plt
import numpy as np
import calendar # calendar 함수를 임포트 해준다.

month_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
sold_list = [300, 400, 550, 900, 600, 960, 900, 910, 800, 700, 550, 450]

fig, ax = plt.subplots(figsize=(10,6))
plt.xticks(month_list, calendar.month_name[1:13], rotation=90)
# .xticks() 는 x축에 눈금을 표시하는 함수입니다.
# .yticks() 는 y축에 눈금을 표시해준다.
plot = ax.bar(month_list, sold_list)

for rect in plot:
  print("graph:", rect) 
  height = rect.get_height()
  ax.text(rect.get_x() + rect.get_width()/2., 1.002*height,'%d' % 
  int(height), ha='center', va='bottom')

plt.show()
```

    graph: Rectangle(xy=(0.6, 0), width=0.8, height=300, angle=0)
    graph: Rectangle(xy=(1.6, 0), width=0.8, height=400, angle=0)
    graph: Rectangle(xy=(2.6, 0), width=0.8, height=550, angle=0)
    graph: Rectangle(xy=(3.6, 0), width=0.8, height=900, angle=0)
    graph: Rectangle(xy=(4.6, 0), width=0.8, height=600, angle=0)
    graph: Rectangle(xy=(5.6, 0), width=0.8, height=960, angle=0)
    graph: Rectangle(xy=(6.6, 0), width=0.8, height=900, angle=0)
    graph: Rectangle(xy=(7.6, 0), width=0.8, height=910, angle=0)
    graph: Rectangle(xy=(8.6, 0), width=0.8, height=800, angle=0)
    graph: Rectangle(xy=(9.6, 0), width=0.8, height=700, angle=0)
    graph: Rectangle(xy=(10.6, 0), width=0.8, height=550, angle=0)
    graph: Rectangle(xy=(11.6, 0), width=0.8, height=450, angle=0)
    


    
![png](/images/visual/output_9_1.png)
    



### 막대 그래프 해석
- 막대형 그래프는 범주 데이터를 요약하는 방법이다.
- 위의 그래프는 1월~12월까지의 개월을 `calendar` 함수를 통해 각 월별 이름을 받아와 월 별 데이터 범주 값을 나타내주고 있다.

## 산점도 그래프 작성법
 - `import seaborn as sns` 은 Matplotlib을 기반으로 다양한 색상 테마와 통계용 차트 등의 기능을 추가한 시각화 패키지이다. 

 - `# sns.scatterplot(x='total_bill', y='tip', data=tips)` 와 같이 각각 선언하지 않고 한번에 하는 것도 가능하다.


```python
import matplotlib.pyplot as plt
import seaborn as sns

# 내장 데이터
tips = sns.load_dataset("tips") # seaborn에서 제공하는 데이터를 불러온다.
x = tips['total_bill']
y = tips['tip']

# sns.scatterplot(x='total_bill', y='tip', data=tips)

fig, ax = plt.subplots(figsize=(10, 6))
ax.scatter(x, y)   # x축과 y축을 그래프에 지정
ax.set_xlabel('Total Bill') # x축 변수명 선언
ax.set_ylabel('Tip') # y축 변수명 선언
ax.set_title('Tip ~ Total Bill') # 그래프 타이틀 선언

fig.show()
```


    
![png](/images/visual/output_13_0.png)
    


### 산점도 그룹화 그래프 작성법
 - `label, data = tips.groupby('sex')`
 - 위 코드를 통해 `tips`의 데이터를 `sex`로 그룹화를 진행해준다.


```python
label, data = tips.groupby('sex')
```


```python
import matplotlib.pyplot as plt
import seaborn as sns

tips['sex_color'] = tips['sex'].map({"Female" : "#0000FF", "Male" : "#00FF00"})
# 남성과 여성의 변수명과 색상을 설정해준다.

fig, ax = plt.subplots(figsize=(10, 6))
for label, data in tips.groupby('sex'):

  # 그룹화한 변수를 불러와 준다.

  ax.scatter(data['total_bill'], data['tip'], label=label, 
             color=data['sex_color'], alpha=0.5)
  
  # scatter를 이용해 점을 찍어준다.
  # alpha=n 은 점의 크기

  ax.set_xlabel('Total Bill')
  ax.set_ylabel('Tip')
  ax.set_title('Tip ~ Total Bill by Gender')

ax.legend()
fig.show()
```


    
![png](/images/visual/output_16_0.png)
    


### 산점도 그래프 해석
- 두 변수 간의 영향력을 보여주기 위해 가로 축과 세로 축에 데이터 포인트를 그리는 데 사용된다.


- 산점도는 두 변수간의 상관 관계를 나타내주는데, 점이 산점도에서 직선에 가까운 경우 두 변수의 상관관계가 높다고 본다.


- 점이 균등하게 분산되어 있는 경우 상관관계가 낮거나 0에 가깝다.


- 두 변수가 다른 변수와 모두 연관될 수 있다. 즉, 우연한 일치로 인해 상관관계가 생성될 수 있다.


- 그러므로 상관관계만으로 인과관계를 장담할 수 없다.


- 산점도는 분석을 할 때 참조선이나 곡선 유형을 
추가하여 일반적으로 사용된다.

## 히스토그램 그래프 작성법
- `seaborn` 에 저장된 `titanic` 데이터 프레임의 `age` 데이터만을 가져온다.


```python
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

# 내장 데이터 
titanic = sns.load_dataset('titanic')
age = titanic['age']


bins = 21
# 가로축 구간 갯수 지정
#nbins = 21 로 작성해도 상관없다.

fig, ax = plt.subplots(figsize=(10, 6))
ax.hist(age, bins = bins)
# 히스토그램의 가로축을 지정해주는 함수 .hist()
ax.set_xlabel("Age")
ax.set_ylabel("Frequency")
ax.set_title("Distribution of Age in Titanic")

ax.axvline(x = age.mean(), linewidth = 2, color = 'r')
# 히스토그램의 평균값을 선으로 나타내주는 함수 .axvline()
fig.show()
```


    
![png](/images/visual/output_19_0.png)
    


### 히스토그램 그래프 해석
- 히스토그램은 <strong color = 'red'>한개</strong>의 변수의 구간별 빈도를 나태난다.
-  위의 표는 `age` 구간별로 탑승객들의 빈도수를 나타내준다. 


## 박스플롯 그래프 작성법



```python
import matplotlib.pyplot as plt
import seaborn as sns

iris = sns.load_dataset('iris')

data = [iris[iris['species']=="setosa"]['petal_width'], 
        iris[iris['species']=="versicolor"]['petal_width'],
        iris[iris['species']=="virginica"]['petal_width']]

fig, ax = plt.subplots(figsize=(10, 6))
ax.boxplot(data, labels=['setosa', 'versicolor', 'virginica'])

fig.show()
```


    
![png](/images/visual/output_22_0.png)
    


### 박스플롯 그래프 해석
- 박스플롯은 데이터의 집합의 범위와 중앙값을 빠르게 확인 할 수 있다. 또, 통계적 이상치 또한 나타내준다.

- 위의 그래프의 경우 `iris`(꽃) 데이터의 `species`(종) 데이터 부분에서 3가지 품종(`setosa`,`versicolor`, `virginica`) 데이터의 수치를 표현한다.

![](/images/visual/boxplot.png)

- 그래프의 각각 의미는
  1. 최댓값
  2. 제 1사분위(Q1)
  3. 중앙값(제 2사분위(Q2))
  4. 제 3사분위(Q3)
  5. 최솟값

  최솟값과 최댓값을 넘어가는 위치의 값은 이상치라 부른다.

## 히트맵
- `matplotlib` 모듈에는 히트맵을 바로 사용할 수 있는 함수가 존재하지 않는다.
- 그럼으로 get_cmap() 를 통해 바로 가져오지 않고 `imshow`() 함수를 활용하여 가져온다.


```python
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

# 내장 데이터
flights = sns.load_dataset("flights")
flights = flights.pivot("month", "year", "passengers")

fig, ax = plt.subplots(figsize=(12, 6))
im = ax.imshow(flights, cmap = 'YlGnBu')
# 히트맵의 색상을 지정해준다.
ax.set_xticklabels(flights.columns, rotation = 20)
ax.set_yticklabels(flights.index, rotation = 10)
fig.colorbar(im)
# 그래프 우측에 구간별 컬러바를 나타내는 함수

fig.show()
```


    
![png](/images/visual/output_25_0.png)
    


### 히트맵 그래프 해석
- 히트맵은 다양한 값을 갖는 숫자 데이터를 열분포 형태와 같이 색상을 이용해 직관적으로 나타낸 그래프다.
- x축과 y축을 범주형 변수로 하고 각 칸에 수치형 변수를 채운다.
- 구체적인 수치가 없어도 많은 데이터의 패턴을 나타내는데 주로 사용된다.
- 위의 데이터의 경우 1950 ~ 1955년 사이 2~7월 사이의 비행기 탑승객의 수를 나타내준다.

# Seaborn
- Seaborn 라이브러를 활용해서 다양한 통계 그래프를 그릴 수 있다.

## 산점도, 회귀선이 있는 선점도 작성법
 -`%matplotlib inline `는 출력옵션의 한 종류로, 이미지, 사운드, 애니메이션 등으로 표현되는 객체를 Jupyter등의 프론트에서 표시되게 하는 기능을 한다.


```python
%matplotlib inline 


import matplotlib.pyplot as plt
import seaborn as sns

tips = sns.load_dataset("tips")
sns.scatterplot(x = "total_bill", y = "tip", data = tips)
plt.show()
```


    
![png](/images/visual/output_29_0.png)
    



```python
fig, ax = plt.subplots(nrows = 1, ncols = 2, figsize=(15, 5))
# nrows = 행, ncols = 열
sns.regplot(x = "total_bill", #x축 지정 
            y = "tip", #y축 지정
            data = tips, 
            ax = ax[0], # 그래프 구분 넘버링
            fit_reg = True) # 회귀선 표시 유무

sns.regplot(x = "total_bill", 
            y = "tip", 
            data = tips, 
            ax = ax[1], 
            fit_reg = False)


plt.show()
```


    
![png](/images/visual/output_30_0.png)
    


### 산점도, 회귀선 그래프 해석
- `norws`는 출력의 행의 갯수, `ncols`는 열의 갯수를 의미한다.


- `ax = ax[]`의 윗줄에 지정한 범위만큼의 값을 넣으면 각 그래프별로 다른 데이터를 넣을 수 있다.


- `regplot()` 함수를 이용해 회귀선을 나타냈다.


- `regplot()` 함수 안에 있는 `fit_reg` 를 `True`로 할 경우 회귀선이 나타나고, `False`로 하면 회귀선이 나오지 않는다. 

## 히스토그램/커널 밀도 그래프


```python
import matplotlib.pyplot as plt
import seaborn as sns

tips = sns.load_dataset("tips")

sns.displot(x = "tip", data = tips)
plt.figure(figsize=(10, 6))
plt.show()
```


    
![png](/images/visual/output_33_0.png)
    



    <Figure size 720x432 with 0 Axes>



```python
sns.displot(x="tip", kind="kde", data=tips)
plt.show()
# 커널 밀도 그래프 추가
```


    
![png](/images/visual/output_34_0.png)
    



```python
sns.displot(x="tip", kde=True, data=tips)
plt.show()
# 위의 두 그래프를 합쳐준다.
```


    
![png](/images/visual/output_35_0.png)
    


## 박스플롯


```python
sns.boxplot(x = "day", y = "total_bill", data = tips)
sns.swarmplot(x = "day", y = "total_bill", data = tips, alpha = .25)
plt.show()
```


    
![png](/images/visual/output_37_0.png)
    


## 막대 그래프


```python
sns.countplot(x = "day", data = tips)
plt.show()
```


    
![png](/images/visual/output_39_0.png)
    



```python
ax = sns.countplot(x = "day", data = tips, order = tips['day'].value_counts().index)
for p in ax.patches:
  height = p.get_height()
  ax.text(p.get_x() + p.get_width()/2., height+3, height, ha = 'center', size=9)
ax.set_ylim(-5, 100)
plt.show()
```


    
![png](/images/visual/output_40_0.png)
    



```python
ax = sns.countplot(x = "day", data = tips, hue = "sex", dodge = True,
              order = tips['day'].value_counts().index)
for p in ax.patches:
  height = p.get_height()
  ax.text(p.get_x() + p.get_width()/2., height+3, height, ha = 'center', size=9)
ax.set_ylim(-5, 100)

plt.show()
```


    
![png](/images/visual/output_41_0.png)
    


