# Float & Flexbox & Bootstrap

---

## Grid System

---

- CSS lay out techniques
  - Display
  - Position
  - Float(CSS1, 1996)
  - Flexbox(2012)
  - Grid(2017)

---

### Float

---

- 박스를 왼쪽 혹은 오른쪽으로 이동시켜 덱스트를 포함 인라인 요소들이 주변을 wrapping 하도록 함
- 요소가 Normal flow를 벗어나도록함
- postioning - absolute: static이 아닌 부모/ fixed: 브라우저



- **Float 속성**
  - none: 기본값
  - left: 왼쪽으로 띄움
  - right: 오른쪽으로 띄움



- **Float 예시**

  ```html
  <body>
      <h1>
          float
      </h1>
      <div class="box left">left</div>
      <div calss="box right">right</div>
      <p>lorem300 자동 완성으로 길~게</p>       
  </body>
  ```

  ```css
  .box {
      width: 150px;
      height: 150px;
      border: 1px solid black;
      background-color: crimson;
      color: white;
      margin-right: 30px;
  }
  
  .left {
      float: left;
  }
  .right {
      float: right;
  }
  ```

  ---

  **clearing**: float를 하면 부모요소의 높이가 0이므로 클리어링을 통해 자식요소 만큼 높이를 가져가게 해준다.

  ```html
  <body>
      <div class="clearfix">
          <div class="box1 left">box1</div>
      </div>
      <div class="box2">box2</div>
  </body>
     
  ```

  ```css
  .box1 {
      width: 150px;
      height: 150px;
      border: 1px solid black;
      background-color: crimson;
      color: black;
      margin-right: 30px;
  }
  .box2 {
      width: 300px;
      height: 150px;
      border: 1px solid black;
      background-color: blue;
      color: black;
      margin-right: 30px;
  }
  
  .left {
      float: left;
  }
  /* ::after는 선택한 요소의 맨 마지막 자식으로 의사 요소를 하나 생성한다. */
  /* content 속성을 통해 자유롭게 텍스트나 이미지를 추가할 수 있다. */
  .clearfix::after {
      content: "";
      display: block;
      clear: both;
  }
  ```

- **Clearing Float**

  - Float는 Normal Flow에서 벗어나 부동상태(떠 있음)
  - 따라서, 이후 요소에 대하여 Float 속성이 적용되지 않도록 Clearing이 필수적임
    - ::after: 선택한 요소의 맨 마지막 자식으로 가상 요소를 하나 생성
      - `::`: 가상요소 선택자 
      - 보통 content 속성과 함께 짝지어, 요소에 장식용 콘텐츠를 추가할 때 사용
    - clear 속성 부여 



- **Float 정리**
  - Float는 레이아웃을 구성하기 위해 필수적으로 활용 되었으며 최근에 사용도가 비교적 낮아짐
  - Float 활용 전략 - Normal Flow에서 벗어난 레이아웃 구성
    - 원하는 요소들을 Float로 지정하여 배치
    - 부모 요소에 반드시 Clearing Float를 하여 이후 요소부터 Normal Flow를 가지도록 지정






---

## Flexbox (CSS Flexible Box Layout)

---

- **Flexbox:** 행과 열 형태로 아이템들을 배치하는 1차원 레이아웃 모델
- **Flexbox 를 사용하는 이유**: 기존의 방법으로는 '수직 정렬'과 '아이템의 너비와 높이 혹은 간격을 동일하게 배치'하는데 어려움이 있었다.

- **축**

  - **main axis** (메인 축)
  - **cross axis** (교차 축): main axis와 수직이 되는 축 

- **구성요소**

  - **Flex Containe**r (부모 요소)

  - **Flex Item** (자식 요소)



---

- **Flexbox 축**

  - 메인 축 방향 설정
  - **flex-direction: row**  - 메인축이 가로 크로스축이 세로
  - **flex-direction: column**  - 메인축이 세로 크로스축이 가로

- **Flexbox 구성요소**

  - **Flex Container (부모요소)**

    - flexbox 레이아웃을 형성하는 가장 기본적인 모델
    - Flex  Item 들이 놓여있는 영역
    - display 속성을 flex 혹은 inline-flex(부모의 속성이 inline인 경우)로 지정

    ```css
    .flex-containger {
        display: flex;
    }
    ```

  - **Flex Item (자식요소)**

    - 컨테이너에 속해있는 컨텐츠 (박스)



---

- **Flex 속성**
  - 축에 따라: **justify** (메인축) / **align** (교차축)
  - 적용대상에 따라: **content** (여러줄) / **items** (한 줄) / **self** (요소하나)
- Flex 속성: **flex-direction**
  - **Main axis 기본 방향 설정**
  - 역방향의 경우 HTML 태그 선언 순서와 시각적으로 다르니 유의
    1. row: 좌에서 우
    2. row-reverse: 우에서 좌
    3. column: 상에서 하
    4. column-reverse: 하에서 상
- Flex 속성: **flex-wrap**
  - 아이템이 컨데이너를 벗어나는 경우 해당 영역 내에 배치되도록 설정
  - 즉, 기본적으로 컨테이너 영역을 벗어나지 않도록 함
    1. wrap: 모두 같은 크기로 차곡차곡 쌓임(넘치면 그 다음 줄로 배치)
       - wrap-reverse: 줄 거꾸로 배치해줌
    2. nowrap(기본값): 한줄에 배치 (가로로 등분되어 세로로 긴 형태로 들어감)
- Flex 속성: **flex-flow**
  - flex-direction과 flex-wrap을 동시에 설정하는 Short-hand
- Flex 속성: **justify-content**
  - **Main axis를 기준**으로 공간배분
    1. start: 남은 영역을 끝에 두어 아이템들을 시작에 정렬
    2. end: 남은 영역을 시작에 두어 아이템들을 끝에 정렬 
    3. center: 남은 영역을 양옆에 등분하여 아이템들을 가운데 정렬
    4. space-between: 남은 영역들을 아이템들 사이사이에 등분하여 정렬
    5. space-around: 남은 영역들을 등분하되 아이템들 양옆으로 영역을 고루 배치(양 끝 영역은 아이템들 사이의 반만큼을 갖게 된다.)
    6. sapce-evenly: 남은 영역들을 양 끝 공간을 포함하여 등분하여 정렬
- Flex 속성: **align-content**
  - **Cross axis를 기준**으로 공간배분 (아이템이 한 줄로 배치되는 경우 확인할 수 없음)
    1. start: 남은 영역을 끝에 두어 아이템들을 시작에 정렬
    2. end: 남은 영역을 시작에 두어 아이템들을 끝에 정렬 
    3. center: 남은 영역을 양옆에 등분하여 아이템들을 가운데 정렬
    4. space-between: 남은 영역들을 아이템들 사이사이에 등분하여 정렬
    5. space-around: 남은 영역들을 등분하되 아이템들 위아래로 영역을 고루 배치(양 끝 영역은 아이템들 사이의 반만큼을 갖게 된다.)
    6. sapce-evenly: 남은 영역들을 양 끝 공간을 포함하여 등분하여 정렬
- Flex 속성: **align-items**
  - 모든 아이템을 **Cross axis를 기준**으로 정렬
    1. stretch: 늘려서 꽉차게 
    2. start: 시작에
    3. end: 끝에
    4. center: 가운데에
    5. baseline: 아이템들끼리 글자 크기가 다를 때도 베이스라인 기준으로 정렬
- Flex 속성: **align-self**
  - 개별 아이템을 **Cross axis 기준**으로 정렬
  - 해당 속성은 컨테이너에 적용하는 것이 아니라 **개별 아이템에 적용**!
    1. stretch: 늘려서 꽉차게 
    2. start: 시작에
    3. end: 끝에
    4. center: 가운데에
- Flex에 적용하는 속성
  - **기타 속성**
    1. **flex-grow**: 남은 영역을 아이템에 분배
       - 남는 것을 배분해주는 것이지 비율로 배분해주는 것이 아님
    2. **order**: 배치순서
       - 기본값이 0이므로 음수를 주면 기본 값보다 앞서고 양수를 주면 뒤에 놓인다.



---

- Flex 활용 레이아웃 - 수직수평 가운데 정렬

  ```css
  .container {
      display: flex;
      justify-content: center
      align-items: center
  }
  
  /* 또는 */
  
  .container {
      margin: auto;
  }
  ```

  

- 활용 레이아웃 - 카드배치

  ```css
  #layout-03 {
      display: flex;
      flex-direction: column;
      flex-wrap: wrap;
      justify-content: center;
   	align-around: space-around;
      align-items: center;   
  }
  ```



---

## Bootstrap (부트스트랩)

---

- 넷플릭스도 부트스트랩으로 만들어진 사이트
- 일반 html과 다르게 설정되어 있다.
  - 예를 들어 h1 태그에서도 margin-top이 없어지고 margin-bottom은 줄어든다.
  - 그 외에도 글씨체도 살짝 바뀌어 있음
- Bootstrap 사이트에서 태그 복사해와야 함
- **사용방법**
  1. **CDN** (content delivery distribution network)
     - 컨텐츠를 효율적으로 전달하기 위해 여러 노드에 가진 네트워크에 데이터를 제공하는 시스텝	

     - 개별 end-user의 가까운 서버를 통해 빠르게 전달 가능(지리적 이점)

     - 외부 서버를 활용함으로써 본인 서버의 부하가 적어짐
     - Get Start에서  CSS 와 JS를 카피해서 쓰기
  2. **로컬에서 직접 css 파일 다운**받아 쓰기



---

- **Spacing**
  - m-0: 0rem (0px)
  - m-1 : 0.25rem (4px)
  - m-2 : 0.5rem (8px)
  - m-3 : 1rem (16px)
  - m-4 : 1.5rem (24px)
  - m-5 : 3rem (48px)
- **margin 클래스**
  - .mt-`n`:  margin-top
  - .mb-`n`:  margin-bottom
  - .ms-`n`:  margin-left
  - .me-`n`:  margin-right
  - .mx-`n`: margin-left, margin-right
  - .my-`n`: margin-top, margin-bottom
  - .mx-auto: 좌우 가운데 정렬
- **padding 클래스**
  - .pt-`n`: padding-top
  - .pb-`n`:  padding-bottom
  - .ps-`n`:  padding-left
  - .pe-`n`:  padding-right
  - .px-`n`: padding-left, padding-right
  - .py-`n`: padding-top, padding-bottom
  - .px-auto: 좌우 가운데 정렬



---

- **Color** (이쁜 색으로 설정해 놓음)
  - .text-[color]: 텍스트 칼라 클래스
  - .bg-[color]: 배경 칼라 클래스
  - .boder-[color]: 테두리 칼라 클래스
  - .alert-[color]: 알림박스 칼라 클래스
  - .btn-[color]: 버튼 칼라 클래스
  - **color 종류**
    - primary: 파란색
    - secondary: 회색
    - sucess: 초록
    - danger: 빨강
    - warning: 노란색
    - info: 터키옥색
    - light: 옅은 회색
    - dark: 검은색



---

### **Responsive Web Design** (반응형 웹)

---

- 다양한 화면 크기를 가진 디바이스들이 등장함에 따라 **responsive web design** 개념 등장
- 반응형 웹은 별도의 기술 이름이 아닌 웹 디자인에 대한 접근 방식, 반응형 레이아웃 작성에 도움이 되는 사례들의 모음 등을 기술하는데 사용되는 용어
- 예시
  - Media Queries, Flexbox, Bootstrap Grid System...



---

- **Grid system (web design)**
  - 요소들의 디자인과 배치에 도움을 주는 시스템
  - **기본 요소**
    - **Column**: 실제 컨텐츠를 포함하는 부분
    - **Gutter**: 칼럼과 칼럼 사이의 공간 (사이 간격)
    - **Container**: Column들을 담고 있는 공간



- **Bootstrap grid System**
  - Bootstrap grid System는 flexbox로 제작됨
  - container, rows, columns 으로 컨텐츠를 구성한다.



- **Grid System breakpoints**
  - xs (extra small): .col-xs
  - sm (small): .col-sm
  - md (midium): .col-md
  - lg (large): .col-lg
  - xl (extra large): .col-xl
  - xxl (extra extra large): .col-xxl



---

- **Grid System 연습하기**

  ```html
  <!--기본형태-->
  <body>
      <div class="container">
          <div class="row">
              <div class="column">1</div>
              <div class="column">2</div>
              <div class="column">3</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--row 여러개 1-->
  <body>
      <div class="container">
          <div class="row">
              <div class="col">1</div>
              <div class="col">2</div>
              <div class="w=100"></div>
              <div class="col">3</div>  
              <div class="col">4</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--row 여러개 2-->
  <body>
      <div class="container">
          <div class="row">
              <div class="col">1</div>
              <div class="col">2</div>
          </div>    
          <div class="row">    
              <div class="col">3</div>
              <div class="col">4</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--column 별로 다른 비율-->
  <!--기본형식의 총 컬럼수: 12개-->
  <!--3개의 컬럼 비율을 1:2:1로 맞추고 싶은 경우-->
  <body>
      <div class="container">
          <div class="row">
              <div class="box col-3">1</div>
              <div class="box col-6">2</div>
              <div class="box col-3">3</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--column이 12개를 넘기는 경우-->
  <body>
      <div class="container">
          <div class="row">
              <div class="box col-1">1</div>
              <div class="box col-1">2</div>
              <div class="box col-1">3</div>
              <div class="box col-1">4</div>
              <div class="box col-1">5</div>
              <div class="box col-1">6</div>
              <div class="box col-1">7</div>
              <div class="box col-1">8</div>
              <div class="box col-1">9</div>
              <div class="box col-1">10</div>
              <div class="box col-1">11</div>
              <div class="box col-1">12</div>
              <div class="box col-1">13</div>
          </div>
      </div>
  </body>
  <!--결과는 13 컬럼이 다음 열로 넘어가게 됨-->
  ```

  ```html
  <!--한 row에 원하는 만큼만 column을 지정해주고 넘어가는 것도 가능하다.-->
  <body>
      <div class="container">
          <div class="row">
              <div class="box col-6">1</div>
          </div>    
          <div class="row">    
              <div class="box col-4">2</div>
              <div class="box col-3">3</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--viewport에 따라 가장 작을 땐 2개, 그 다음은 3개, 그 다음은 4개-->
  <body>
      <div class="container">
          <div class="row">
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
              <div class="box col-6 col-sm-4 col-md-3">1</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--Nesting-->
  <!--level 안에 level 넣기-->
  <body>
      <div class="container">
          <div class="box col-6">
              <div class="row">
                  <div class="box col-3">1</div>
                  <div class="box col-3">2</div>
                  <div class="box col-3">3</div>
                  <div class="box col-3">4 </div>
              </div>
          </div>
          <div>
              <div class="box col-6"></div>
              <div class="box col-6"></div>
              <div class="box col-6"></div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--Offset-->
  <!--띄우기(margin)-->
  <body>
      <div class="container">
          <div class="row">
              <div class="box offset-2 col-3"></div>
              <div class="box col-3">2</div>
              <div class="box col-3">3</div>
          </div>
      </div>
  </body>
  ```

  ```html
  <!--Offset-->
  <!--띄우기(margin)-->
  <!--breakpoint 적용-->
  <body>
      <div class="container">
          <div class="row">
              <div class="box col-md-4 offset-4">md-4 offset-4</div>
              <div class="box col-md-4 offset-md-4">md-4 offset-md-4</div>
          </div>
      </div>
  </body>
  ```



- 기타 클래스

  - border-width

  - border-radius
    - rounded
    - rounded-top
    - rounded-end
    - rounded-bottom
    - rounded-circle
    - rounded-start
    - rounded-pill

  - card

  - 등등은 공식 문서에서 확인하면  됨