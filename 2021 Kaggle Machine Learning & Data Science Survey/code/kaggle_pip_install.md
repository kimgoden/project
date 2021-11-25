---
title: kaggle pip install 하는법
date: 2021-11-07 20:03:16
categories:
- 캐글
- 캐글 셋팅
tags:
- kaggle
- python
- plotly
- 시각화
---

# 캐글에서 pip install 하는 법
- 캐글 notebook 에서 모듈을 인스톨할라고 하면 <br>`WARNING: Retrying (Retry(total=0, connect=None, read=None, 
redirect=None, status=None)) after connection broken by 'NewConnectionError
('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x7f7684da26d0>: Failed to establish a new connection:
[Errno -3] Temporary failure in name resolution')': /simple/circlify/`</br>
이라는 오류가 뜨면서 설치가 안된다.

-이럴 경우 notebook의 설정을 바꿔주어 해결 할 수 있었다.(웨일, 크롬에서 작동 확인함)
![](/images/plotly/install_er.png)
위의 이미지 처럼 Settings-internet에서 off가 되어 있다면 on 으로 바꿔줄 경우 정상적으로 인스톨이 가능하다.

- 필자의 경우 처음 Settings을 들어갔을때 internet 란이 없었다. 
- 그 경우 Settings 하단 인증 부분에 들어가 휴대전화 문자 인증 후, internet 설정이 가능 했다.



출처: https://somjang.tistory.com/entry/Kaggle-Notebook-에서-라이브러리-설치-방법 [솜씨좋은장씨]
