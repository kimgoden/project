# 2021 DACON Corporate Trend Visualization Competition

## 워크넷 오픈 API를 활용한 채용공고 분석 % IT직종 세부 분석

## 1. 프로젝트 개요

[구직자를 위한 기업 트렌드 시각화 경진대회](https://dacon.io/competitions/official/235866/overview/description)

### 1) 대회 개요

구직자가 회사를  접하기 힘들기에 많은 구직자가 회사를 선택하기 어려움을 겪고 있다.

**따라서 본 대회에서는 구직자를 위한** 선택하는 기준에는 임금, 안정성, 복지, 성장 가능성 등의 다양한 요인이 있다.

하지만 이런 요인들은 구직자들이 쉽게 **기업 트렌드 정보(구직자가 회사를 선택하는 다양한 기준)를 빅데이터 시각화로 구현하고,**

**정보 전달의 효율성을 기대할 수 있는 아이디어 및 인사이트를 발굴하고자 한다.**

 

### 2) 목적

1. 기업 트렌드를 확인할 수 있는 비정형&정형 데이터 발굴
2. 구직자가 필요로 하는 기업 트렌드 아이디어 및 인사이트 발굴

### 3)주최 /  주관

- 주최 : 한국고용정보원
- 주관 : 데이콘

### 4)사용 데이터

- 워크넷 오픈 API 채용공고.XML
- 워크넷 제공 직종분류.csv
- 워크넷 오픈 API 공채정보.XML


### 분석 방향
1. 구직자가 구직에 필요한 정보를 제공하기 위해 정형&비정형 데이터를 발굴 및 추출하여 시각화를 통해 정보를 전달한다.

2. 오픈소스를 활용, 동시에 상호작용 가능한 시각화를 통해 별도의 대시보드가 아니라도 직접 차트를 조절함으로서 유저가 선택적으로 정보를 수집 할 수 있게 돕는다.

3. 단순히 워크넷에서 제공하는 정보 뿐만 아니라 기존 제공하는 데이터를 가공 및 추가 정보를 도출하여 구직자에게 실질적인 정보를 전달 할 수 있게 한다.  

4. 주요 관심 직군인 IT 직군에 대한 세부적인 분석을 통해 최근 IT업계의 현 주소를 파악 할 수 있게 돕는다.

<hr>


### 분석 자료 작성
1. 분석 개요
- 데이콘의 배경적 지식과 대회의 의의를 설명하고 구직자에게 효과적인 정보 전달법을 모색
- 포스트 코로나 시대에 이전과 달라진 취업시장에 대해 조사하고 이용자에게 필요한 데이터를 서치
- 인사이트 발굴을 위한 주요 키워드를 크게 3가지로 설정
  - 다양한 정보 전달
  - 높은 접근성
  - 직관적 분석

2. 분석 목적과 진행과정
- 해당 대회는 별도 데이터소스가 제공되지 않고 참가자가 효과적인 정보 전달을 위해 데이터를 발굴 하는 것 부터 시작이다.
- 직접적인 채용정보를 획득하기 위해 다양한 채용 플랫폼을 선정
  - 그 중 한국고용정보원에서 운영하는 국내 최대 취업 플랫폼 <strong>워크넷</strong> 선택
- 매일 수많은 양의 채용정보들이 업로드 및 오픈 API를 통해 이용자에게 제공
- 다양한 파라미터를 통해 선별적으로 데이터에 접근이 가능
- 별도의 상용 툴을 사용하지 않고 반응형 시각화를 진행하기 위해 [Pyecharts](https://pyecharts.org/#/) 툴 선정
- 다양한 커스터마이징을 통해 직관성을 확보하고 이용자로 하여금 선택적인 정보를 습득할 수 있게 시각화를 진행
  <hr>

3. 시각화 결과물 예시
  - 지역별 채용공고 분포에 대한 분석 시각화
![지역별 채용공고](https://user-images.githubusercontent.com/93235480/153355894-fb794297-8729-486f-ab94-35ca125289f1.png)
  - 하단 슬라이드 바를 이용한 접근 시각화
![it 상세직군별 급여](https://user-images.githubusercontent.com/93235480/153355903-fc6a4ef6-68dc-4527-b601-1f389421a908.png)
  - 워드 클라우드 시각화 
![워드클라우드](https://user-images.githubusercontent.com/93235480/153355825-01002311-3b9f-4dcd-af6e-588ae2691c32.png)

    
<hr>

### 최종 결과물 작성
- [데이콘 제출 코드](https://github.com/kimgoden/project/blob/main/2021%20DACON%20Corporate%20Trend%20Visualization%20Competition/code/%EB%8D%B0%EC%9D%B4%EC%BD%98_%EC%9B%8C%ED%81%AC%EB%84%B7API_%EC%B1%84%EC%9A%A9%EA%B3%B5%EA%B3%A0_%EB%B6%84%EC%84%9D_%EC%B5%9C%EC%A2%85%EC%A0%9C%EC%B6%9C.ipynb)
- [최종 발표 자료](https://github.com/kimgoden/project/blob/main/2021%20DACON%20Corporate%20Trend%20Visualization%20Competition/docs/%EB%8D%B0%EC%9D%B4%EC%BD%98_%EA%B5%AC%EC%A7%81%EC%9E%90%EB%A5%BC%20%EC%9C%84%ED%95%9C%20%EC%8B%9C%EA%B0%81%ED%99%94%20%EA%B2%BD%EC%A7%84%20%EB%8C%80%ED%9A%8C.pdf)
<hr>

### 분석 데이터 다운로드
- [워크넷_채용정보](https://github.com/kimgoden/project/blob/main/2021%20DACON%20Corporate%20Trend%20Visualization%20Competition/data/%EC%9B%8C%ED%81%AC%EB%84%B7_%EC%B1%84%EC%9A%A9%EC%A0%95%EB%B3%B4_2022_01_19.csv)
- [직종 코드에 따른 직종 분류](https://github.com/kimgoden/project/blob/main/2021%20DACON%20Corporate%20Trend%20Visualization%20Competition/data/%EC%A7%81%EC%A2%85%EC%BD%94%EB%93%9C.csv)
- [워크넷_공채속보](https://github.com/kimgoden/project/blob/main/2021%20DACON%20Corporate%20Trend%20Visualization%20Competition/data/%EC%A0%95%EA%B7%9C%EC%A7%81_%EA%B3%B5%EC%B1%84%EC%86%8D%EB%B3%B4.csv)
