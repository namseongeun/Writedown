# Python 기초

## python의 문법

### 저장/ 조건 / 반복

---

1. **저장: 등호를 사용하여 오른쪽의 내용을 왼쪽의 변수에 할당**

- **무엇을 저장하는가?**
  - **숫자(int)**
    - 현실세계에 존재하는 모든 숫자
    - 기본적인 연산이 가능
  - 글자(str)
    - 현실세계에 존재하는 모든 글자
    - 따옴표 `"", ''`로 둘러싼다.
  - **참/거짓**
    - True, False
    - 조건과 반복에 사용한다.
- **어떻게 저장하는가?**
  - 변수(variable)의 형태: 등호`=` 사용
  - 리스트(list)의 형태: 대괄호`[]`사용 (타 언어에서의 array와 동일한 개념이다.)
  - 딕셔너리(dict)의 형태: 중괄호`{}` 사용 (key: value 형태로 저장한다.)

---

2. **조건문(if / elif / else)**

   - `if 조건:`

     ​		`명령문``

     `elif 조건:`

     ​		`명령문`

     `else:`

     ​		`명령문`

---

3. **반복문(while / for)**
   - while문 : while의 조건이 만족될 때까지 반복
     - 종료조건이 반드시 필요!
   - for문: for의 범위 내에서 반복

---

### 함수와 모듈

---

- **함수**
  - Built-in Functions (내장함수) 
  - Non-built-in Functions
- **모듈**
  - 함수들의 모음

---

### 웹크롤링(Web Crawling)

---

- **import requests**

1. requests.get('주소') : 주소로 요청을 보냄 (브라우저의 검색과 동일한 행위)
2. requests.get('주소').text : 주소로부터 텍스트만 받겠다.
3. requests.get('주소').status_code : 주소로부터 상태 코드만 받겠다.



- **from bs4 import BeautifulSoup**

1. BeautifulSoup(문서) : 문서를 보기 좋게 만들기
2. BeautifulSoup.select(selector) : 문서 내의 특정 내용 추출
   - .select_one(selector): 하나만 추출
   - selector(선택자)는 HTML로부터 원하는 결과를 가져오기 위한 것