# JavaScript 기초

## JavaScript Intro

### JavaScript의 필요성

- 브라우저 화면을 '동적'으로 만들기 위함
- **브라우저를 조작할 수 있는 유일한 언어**



### 브라우저 (browser)

---

- URL로 웹(WWW)을 탐색하며 서버와 통신하고, HTML 문서나 파일을 출력하는 GUI기반의 소프트웨어
- 인터넷의 컨텐츠를 검색 및 열람하도록 함
- '웹 브라우저'라고도 함



#### 브라우저에서 할 수 있는 일

- DOM(Document Object Model) 조작
  - 문서(HTML) 조작
- BOM(Browser Object Model) 조작
- JavaScript Core (ECMAScript)로 조작



**DOM (Document Object Model)**

- HTM L, XML과 같은 **문서를 다루기 위한 프로그래밍 인터페이스**
- 문서를 구조화하고, 구조화된 구성 요소를 하난의 객체로 취급하여 다루는 논리적 트리 모델
- 문서가 객체(object)로 구조화되어 있으며 key로 접근 가능
- 단순한 속성 접근, 메서들 활용뿐만 아니라 프로그래밍 언어적 특성을 활용한 조작이 가능



- **DOM - 해석**
  - 파싱 (Parsing)
    - 구문 분석, 해석
    - 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정



**BOM (Browser Object Model)**

- **자바스크립트가 브라우저와 소통하기 위한 모델**
- 브라우저의 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
  - 버튼, URL 입력창, 타이틀 바 등 브라우저 윈도우 및 웹페이지 일부분을 제어 가능 
- window 객체는 모든 브라우저로부터 지원받으며 부라우저의 창을 지칭하나.



**JavaScript Core**

- 브라우저(BOM & DOM)을 조작하기 위한 명령어 약속(언어)



**브라우저 정리**

- **브라우저(BOM)과 그내부의 문서(DOM)를 조작하기 위해 ECMAScript(JS)를 학습**



### JavaScript Intro

---

#### 세미콜론 (semicolon)

- 자바스크립트는 세미콜론을 선택적으로 사용 가능
- 세미콜론이 없으면 ASI에 의해 자동으로 세미콜론이 삽입됨



#### 코딩 스타일 가이드

- 코딩 스타일의 핵심은 합의된 원칙과 일관성
- 코딩 스타일은 코드의 품질에 직결되는 중요한 요소
  - 코드의 가독성, 유지보수 또는 팀원과의 커뮤니케이션 등 개발 과정 전체에 영향을 끼친다.



## 변수

### 변수와 식별자

---

#### 식별자

- 식별자 정의와 특징

  - **식별자는 변수를 구분할 수 있는 변수명을 말한다.**

  - 식별자는 반드시 문자, 달러 또는 밑줄로 시작한다.

  - 대소문자를 구분하며 클래스명 외에는 모두 소문자로 시작한다.

  - 예약어 사용 불가능

  - 식별자 작성 스타일
    - **카멜 케이스**: 변수, 객체, 함수에 사용
      - 두 번째 단어의 첫 글자부터 대문자로 작성
      - 예: camelCase
    - **파스칼 케이스:** 클래스, 생성자에 사용
      - 모든 단어의 첫 번째 글자를 대문자로 작성
      - 예: PascalCase
    - **대문자 스네이크 케이스**: 상수에 사용
      - 모든 단어 대문자 작성 & 단어 사이에 언더스코어 삽입
      - 예: SNAKE_CASE
      - 상수: 개발자의 의도와 상관없이 변결될 가능성이 없는 값을 의미



#### 변수 선언 키워드

**let**

- 재할당 할 예정인 변수 선언 시 사용

  ```javascript
  let number = 10		// 1. 선언 및 초기값 할당
  number = 10			// 2. 재할당
  
  console.log(number) // 10
  ```

- 변수 재선언 불가능

  ```javascript
  let number = 10		// 1. 선언 및 초기값 할당
  let number = 50		// 2. 재선언 불가능
  ```

- 블록 스코프(block scope)

  - if, for, 함수 등의 중괄호 내부를 가리킴

  - 블록 스코프를 가지는 변수는 블록 바깥에서 접근 불가능

    ```javascript
    let x = 1
    
    if (x === 1) {
        let x = 2
        console.log(x)	// 2
    }
    
    console.log(x)		// 1
    ```



**const**

- 재할당 할 예정이 없는 변수 선언 시 사용

  ```javascript
  const number = 10	// 1. 선언 및 초기값 할등
  number = 10			// 2. 재할당 불가능
  ```

- 변수 재선언 불가능

  ```javascript
  const number = 10		// 1. 선언 및 초기값 할당
  const number = 50		// 2. 재선언 불가능
  ```

- 불록 스코프



**[참고] 선언, 할당, 초기화**

```javascript
let foo				// 선언
console.log(foo)	// undefined

foo = 11			// 할당
console.log(foo)	// 11	

let bar = 0			// 선언 + 할당
console.log(bar)	// 0

```

- **선언(Declaration)**
  - 변수를 생성하는 행위 또는 시점
- **할당(Assignment)**
  - 선언된 변수에 값을 저장하는 행위 또는 시점
- **초기화(Initialization)**
  - 선언된 변수에 처음으로 값을 저장하는 행위 또는 시점



**var**

- var로 선언한 변수는 재선언 및 재할당 모두 가능

- 호이스팅 되는 특성으로 인해 예기치 못한 문제가 발생하므로 사용하지 않는 것을 권장

- **함수 스코프**

  ```javascript
  function foo() {
      var x = 5
      console.log(x) // 5
  }
  
  console.log(x)
  ```

  - 함수의 중괄호 내부를 가리킴
  - 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가능

- **호이스팅(hoisting)**

  - 변수를 선언 이전에 참조할 수 있는 현상
  - 변수 선언 이전의 위치에서 접근 시 undefined를 반환



## 데이터 타입

### 데이터 타입 종류

---

- 자바스크립트의 모든 값은 특정한 데이터 타입을 가짐
- 크게 **원시타입**과 **참조타입**으로 분류



#### 원시타입 (Primitive type)

- Number, String, Boolean, undefined, null, Symbol

- 객체(object)가 아닌 기본 타입

- 변수에 해당 타입의 값이 담김

- 다른 변수에 복사할 때 실제 값이 복사됨

  ```javascript
  let messege = '안녕하세요!'	 // 1. message 선언 및 할당
  
  let greeting = message		// 2. greeting에 message 복사
  console.log(greeting)		// 3. '안녕하세요!' 출력
  
  message = 'Hello, world!'	// 4. message 재할당
  console.log(greeting)		// 5. '안녕하세요!' 출력
  
  // 즉, 원시 타입은 실제 해당 타입의 값을 변수에 저장한다.
  ```



**숫자(number) 타입**

- 정수, 실수 구분 없는 하나의 숫자 타입

- 부동소수점 형식을 따른다.

  ```javascript
  const a = 13			// 양의 정수
  const b = -5			// 음의 정수
  const c = 3.14			// 실수
  const d = 2.998e8		// 거듭제곱
  const e = Infinity		// 양의 무한대
  const f = -Infinity		// 음의 무한대
  const g = NaN			// 산술 연산 불가
  ```

- [참고] NaN (Not - A - Number)

  - 계산 불가능한 경우 반환되는 값



**문자열(string) 타입**

- 텍스트 데이터를 나타내는 타입

- 16비트 유니코드 문자의 집합

- 작은따옴표 또는 큰따옴표 모두 가능

- 템플릿 리터럴 (Template Literal)

  - ES6부터 지원
  - 따옴표 대신 백틱으로 사용
  - ${ expression } 형태로 표현식 삽입 가능

  ```javascript
  const firstName = 'Brandan'
  const lastName = 'Eich'
  const fullName = `${firstName} ${lastName}`
  
  console.log(fullName) // Brandan Eich
  ```



**undefined**

- 변수의 값이 없음을 나타내는 데이터 타입

- 변수 선언 이후 직접 값을 할당하지 않으면, 자동으로 undefined가 할당됨

  ```javascript
  let firstName
  console.log(firstName) // undefined
  ```



**null**

- 변수의 값이 없음을 '의도적'으로 표현할 때 사용하는 데이터 타입

  ```javascript
  let firstName = null
  console.log(firstName) // null
  
  typeof null // object
  ```



**[참고] undefined vs null**

- undefined
  - 빈 값을 표현하기 위한 데이터 타입
  - 변수 선언 시 아무 값도 할당하지 않으면, 자바스크립트가 자동으로 할당
  - typeof 연산자의 결과는 undefined
- null
  - 빈 값을 표현하기 위한 테이터 타입
  - 개발자가 의도적으로 필요한 경우 할당
  - typeof 연산자의 결과는 object



**Boolean**

- 논리적 참 또는 거짓을 나타내는 타입

- true 또는 false로 표현

- 조건문 또는 반복문에서 유요하게 사용

  ```javascript
  let isAdmin = true
  console.log(isAdmin) // true
  
  isAdmin = false
  console.lgo(isAdmin) // false
  ```

- [참고] ToBoolean Conversions 정리

  |  데이터 타입  |    거짓    |        참        |
  | :-----------: | :--------: | :--------------: |
  | **Undefinde** | 항상 거짓  |        x         |
  |   **Null**    | 항상 거짓  |        x         |
  |  **Number**   | 0, -0, NaN | 나머지 모든 경우 |
  |  **String**   | 빈 문자열  | 나머지 모든 경우 |
  |    Object     |     x      |     항상 참      |



#### 참조타입 (Reference type)

- Array, Function, ...

- 객체(object) 타입의 자료형

- 변수에 해당 객체의 참조 값이 담김

- 다른 변수에 복사할 때 참조 값이 복사됨

  ```javascript
  const messege = '안녕하세요!'	 // 1. message 선언 및 할당
  
  const greeting = message		// 2. greeting에 message 복사
  console.log(greeting)		// 3. ['안녕하세요!'] 출력
  
  message = 'Hello, world!'	// 4. message 재할당
  console.log(greeting)		// 5. ['Hello, world!'] 출력
  
  // 즉, 참조 타입은 해당 객체를 참조할 수 있는 참조 값을 저장한다.
  ```



## 연산자

#### 할당 연산자

- 오른쪽에 있는 피연산자의 평가 결과를 왼쪽 피연산자에 할당하는 연산자

- 다양한 연산에 대한 단축 연산자 지원

  ```javascript
  let x = 0
  
  x += 10
  console.log(x) // 10
  
  x -= 3
  console.log(x) // 7
  
  x *= 10
  console.log(x) // 70
  
  x /= 10
  console.log(x) // 7
  
  x++
  console.log(x) // 8
  
  x--
  console.log(x) // 7
  ```

  - [참고] Increment 및 Decrement 연산자
    - Increment(++): 피연산자의 값을 1 증가시키는 연산자
    - Dcrement(--): 피연산자의 값을 1감소시키는 연산자



#### 비교 연산자

- 피연산자들(숫자, 문자, Boolean 등)을 비교하고 결과값을 boolean으로 반환하는 연산자

- 문자열은 유니코드 값을 사용하며 표준 사전 순서를 기반으로 비교

  ```javascript
  const numOne = 1
  const numTwo = 100
  console.log(numOne < numTwo) 	// true
  
  const charOne = 'a'
  const charTwo = 'z'
  console.log(charOne > charTwo) 	// false
  ```

  

#### 동등 비교 연산자 (==)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 반환

- 비교할 때 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교

- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별

- 예상치 못한 결과가 발생할 수 있으니 특별한 경우를 제외하고 사용하지 않는다.

  ```javascript
  const a = 1004
  const b = '1004'
  console.log(a == b) // true
  
  const c = 1
  const d = true
  console.log(c == d) // true
  
  // 자동 타입 변환 예시
  console.log(a + b)	 // 10041004
  console.log(c + d)	 // 2
  ```

  

#### 일치 비교 연산자 (===)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 반환

- 엄격한 비교가 이뤄지며 암묵적 타입 변환이 발생하지 않음

  - 엄격한 비교: 두 비교 대상의 타입과 값 모두 값은지 비교하는 방식

- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별

  ```javascript
  const a = 1004
  const b = '1004'
  console.log(a === b) // false
  
  const c = 1
  const d = true
  console.log(c === d) // false
  ```

  

#### 논리 연산자

- 세 가지 논리 연산자로 구성

  - and 연산은 `&& ` 연산자를 이용
  - or 연산은 `||` 연산자를 이용
  - not 연산은 `!` 연산자를 이용

- 단축 평가 지원

  ```javascript
  /* and 연산 */
  console.log(true && false)	// false
  console.log(true && true)	// true
  console.log(1 && 0)			// 0
  console.log(4 && 7)			// 7
  console.log('' && 5)		// ''
  
  /* or 연산 */
  console.log(true || false)	// true
  console.log(false || false)	// false
  console.log(1 || 0)			// 1
  console.log(4 || 7)			// 4
  console.log('' || 5)		// 5
  
  /* not 연산 */
  console.log(!true)			// false
  console.log(!'Bonjour!')	// false
  ```

  

#### 삼항 연산자(Ternary Operator)

- 세 개의 피연산자를 사용하여 조건에 따라 값을 반환하는 연산자

- 가장 왼쪽의 조건식이 참이면 콜론(:) 앞의 값을 사용하고 그렇지 않으면 콜론(:) 뒤의 값을 사용

- 삼항 연산자의 결과 값이기 때문에 변수에 할당 가능

- 한 줄에 표기하는 것을 권장한다.

  ```javascript
  console.log(true ? 1 : 2) // 1
  console.log(false ? 1 : 2) // 2
  
  const result = Maht.PI > 4 ? 'Yes' : 'No'
  console.log(result) // No
  ```

  



## 조건/반복

### 조건문

---

#### 조건문의 종류와 특징

**'if' statement**

- 조건 표현식의 결과값을 Boolean 타입으로 변환 후 참/ 거짓을 판단



- **if , else if , else**

  - 조건은 소괄호 안에 작성

  - 실행할 코드는 중괄호 안에 작성

  - 블록 스코프 생성

    ```javascript
    if (condition) {
        // do something
    } else if (condition) {
        // do something
    } else {
        // do something
    }
    ```

    ```javascript
    const nation = 'Korea'
    
    if (nation === 'Korea') {
        console.log('안녕하세요!')
    } else if (nation === 'France') {
        console.log('Bonjour!')
    } else {
        console.log('Hello!')
    }
    ```

    

**'switch' statement**

- 조건 표현식의 결과값이 어느 값(case)에 해당한는지 판별
- 주로 특정 변수의 값에 따라 조건을 분기할 때 활용
  - 조건이 많아질 경우 if 문보다 가독성이 나을 수 있음



- **switch**

  - 표현식(expression)의 결과값을 이용한 조건문

  - 표현식의 결과값과 case문의 오른쪽 값을 비교

  - break 및 default문은 선택적으로 사용 가능

  - break문이 없는 경우 break문을 만나거나 default문을 실행할 때까지 다음 조건문 실행

  - 블록 스코프 생성

    ```javascript
    switch(expression) {
        case 'first value': {
            // do something
            break
        }
        case 'second value': {
            // do something
            break
        }
        defalut: {
         	// do something
        }
    }
    ```

    ```javascript
    const nation = 'Korea'
    
    switch(nation) {
        case 'Korea': {
            console.log('안녕하세요!')
            break
        }
        case 'France': {
            console.log('Bonjour')
            break
        }
        defalut: {
         	console.log('Hello')
        }
    }
    ```



### 반복문

---

#### 반복문의 종류와 특징

**while**

- 조건문이 참(true)인 동안 반복 시행

- 조건은 소괄호 안에 작성

- 실행할 코드는 중괄호 안에 작성

- 블록 스코프 생성

  ```javascript
  while (condition) {
      // do something
  }
  ```

  ```javascript
  let i = 0
  
  while (i < 6) {
      console.log(i) // 0, 1, 2, 3, 4, 5
      i += 1
  }
  ```

  

**for**

- 세미콜론으로 구분되는 세 부분으로 구성

  - **initialization**

    - 최초 반복문 진입 시 1회만 실행되는 부분

  - **condition**

    - 매 반복 시행전 평가 되는 부분

  - **expression**

    - 매 반복 시행 이후 평가 되는 부분

  - 블록 스코프 생성

    ```javascript
    for (initialization; condition; expression) {
        // do something
    }
    ```

    ```javascript
    for (let i = 0; i < 6; i++) {
        console.log(i) // 0, 1, 2, 3, 4, 5
    }
    
    // 1. 반복문 진입 및 변수 i 선언
    // 2. 조건문 평가 후 코드 블럭 실행
    // 3. 코드 블록 실행 이후 i값 증가
    ```

    ```javascript
    for (const i = 0; i < 6; i++) {
        console.log(i) // 오류발생
    }
    
    // 반복되면서 재할당이 되기 때문이다.
    // 어떤 목적을 갖고 사용하는 것이 아니라 임의변수가 필요한 경우에는 let 사용
    ```

    

- **for ... in**

  - 주로 객체(object)의 속성들을 순회할 때 사용

  - 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 없으므로 권장하지 않음(순회 시에 순서보장이 안됨)

  - 실행할 코드는 중괄호 안에 작성

  - 블록 스코프 생성

    ```javascript
    for (variable in object) {
        // do something
    }
    ```

    ```javascript
    // object(객체) => key-value로 이루어진 자료구조(객체 챕터에서 학습 예정)
    const capitals = {
        korea: 'seoul',
        france: 'paris',
        USA: 'wahsington D.C.'
    }
    
    for (let capital in capitals) {
        console.log(capital) // korea, france, USA
    }
    ```

    

- **for ... of**

  - 반복 가능한(iterable)객체를 순회하며 값을 꺼낼 때 사용

    - 반복 가능한 객체의 종류: Array, Map, Set, String 등

  - 실행할 코드는 중괄호 안에 작성

  - 블록 스코프 생성

    ```javascript
    for (variable of iterables) {
        // do something
    }
    ```

    ```javascript
    const fruits = ['딸기', '바나나', '메론']
    
    for (let fruit of fruits) {
        fruit = fruit + '!'
        console.log(fruit)
    }
    
    for (const fruit of fruits) {
        // fruit 재할당 불가
        console.log(fruit)
    }
    ```



#### 조건문과 반복문 정리

|     키워드     |  종류  |      연관 키워드      |   스코프    |
| :------------: | :----: | :-------------------: | :---------: |
|     **if**     | 조건문 |           -           | 블록 스코프 |
|   **switch**   | 조건문 | case, break, default  | 블록 스코프 |
|   **while**    | 반복문 |    break, continue    | 블록 스코프 |
|    **for**     | 반복문 |    break, continue    | 블록 스코프 |
| **for ... in** | 반복문 |       객체 순회       | 블록 스코프 |
| **for ... of** | 반복문 | 배열 등 Iterable 순회 | 블록 스코프 |



## 함수

### 함수 in JavaScript

- 참조 타입 중 하나로써 function 타입에 속함
- JavaScript에서 함수를 정의하는 방법은 주로 2가지로 구분
  - **함수 선언식**(function declaration)
  - **함수 표현식**(function expression)
- [참고] JavaScript의 함수는 일급 객체(First-class citizen)에 해당한다.
  - 일급객체: 담음의 조건들을 만족하는 객체를 의미한다.
    - 변수에 할당 가능
    - 함수의 매개변수로 전달 가능
    - 함숭의 반환값으로 사용 가능
- **함수는 결국 값으로써 사용**



#### 함수 선언식

- **함수의 이름과 함께 정의하는 방식**

- 3가지 부분으로 구성

  - **함수의 이름(name)**
  - **매개변수 (args)**
  - **몸통 (중괄호 내부)**

  ```javascript
  function name(args) {
      //do something
  }
  ```

  ```javascript
  function add(num1, num2) {
      return num1 + num2
  }
  
  add(1, 2)
  ```



#### 함수 표현식

- **함수를 표현식 내에서 정의하는 방식**

  - 표현식: 어떤 하나의 값으로 결정되는 코드의 단위

- 함수의 이름을 생략하고 **익명 함수로 정의 가능**

  - 익명 함수(anonymous function): 이름이 없는 함수
  - 익명 함수는 함수 표현식에서만 가능

- 3가지 부분으로 구성

  - **함수의 이름 (생략)**
  - **매개변수 (args)**
  - **몸통 (중괄호 내부)**

  ```javascript
  const name = function(args) {
      // do something
  }
  ```

  ```javascript
  const add = function (num1, num2) {
      return num1 + num2
  }
  
  add(1, 2)
  ```



#### 기본 인자 (default arguments)

- 인자 작성 시 '=' 문자 뒤 기본 인자 선언 가능

  ```javascript
  const greeting = function (name = 'Anonymous') {
      return `Hi ${name}`
  }
  
  greeting()	// Hi Anonymous
  ```



#### 매개변수와 인자의 개수 불일치 허용

- 매개변수보다 인자의 개수가 많을 경우,

  ```javascript
  const noArgs = function () {
      return 0
  }
  
  noArgs(1, 2, 3)		// 0
  
  const twoArgs = function (arg1, arg2) {
      return [arg1, arg2]
  }
  
  twoArgs(1, 2, 3)	// [1, 2]
  ```

- 매개변수보다 인자의 개수가 적을 경우,

  ```javascript
  const threeArgs = function(arg1, arg2, arg3) {
      return [arg1, arg2, arg3]
  }
  
  threeArgs() 	// [undefined, undefined, undefined]
  threeArgs(1)	// [1, undefined, undefined]
  threeArgs(1,2)	// [1, 2, undfined]
  ```



#### Rest Parameter

- rest parameter(...)를 사용하면 함수가 정해지지 않은 수의 매개변수를 배열로 받음 (python의 *args와 유사)

  - 만약 rest parameter로 처리간 매개변수에 인자가 넘어오지 않을 경우에는, 빈 배열로 처리

  ```javascript
  const restOpr = function (arg1, arg2, ...restArgs) {
      return [arg1, arg2, restArgs]
  }
  
  restOpr(1, 2, 3, 4, 5)	// [1, 2, [3, 4, 5]]
  restOpr(1, 2)			// [1, 2, []]
  ```



#### Spread Operator

- spread operator(...)를 사용하면 배열 인자를 전개하여 전달 가능

  ```javascript
  const spreadOpr = function (arg1, arg2, arg3) {
      return arg1 + arg2 + arg3
  }
  
  const numbers = [1,2,3]
  spreadOpr(...numbers)	// 6
  ```

  ```javascript
  const spreadOpr = function (arg1, arg2, arg3, arg4) {
      return arg1 + arg2 + arg3 + arg4
  }
  
  const numbers = [1,2,3]
  console.log(spreadOpr(1, ...numbers))	// NaN
  // arg4가 연산이 불가능한 값이기 때문
  ```



#### [참고] Rest Parameter과 Spread Operator 비교

- **Rest Parameter**
  - 함수의 선언문의 파라미터에 (...) 이용해서 받으면 가변인자를 받아 배열로 만들어서 사용하는 것
- **Spread Operator**
  - 함수 호출문의 파라미터에 (...)을 이용해서 호출하면 배열이 해당 매개변수로 각각 매핑되는 것



#### [참고] 함수 선언식과 표현식 비교

|            | 함수 선언식                   | 함수 표현식                   |
| ---------- | ----------------------------- | ----------------------------- |
| **공통점** | 데이터 타입, 함수 구성 요소   | 데이터 타입, 함수 구성요소    |
| **차이점** | 익명 함수 불가능, 호이스팅 됨 | 익명 함수 가능, 호이스팅 안됨 |
| **비고**   |                               | 권장방식                      |



- **함수의 타입**

  - 선언식 함수와 표현식 함수 모두 **타입은 function**으로 동일

    ```javascript
    // 함수 표현식
    const add = function (args) { }
    
    // 함수 선언식
    function sub(args) { }
    
    console.log(typeof add) // function
    console.log(typeof sub) // function
    ```

- **호이스팅 (hoisting) - 함수 선언식**

  - **함수 선언식으로 선언한 함수는 var로 정의한 변수처럼 hoisting 발생**

  - 함수 호출 이후에 선언해도 동작

    ```javascript
    add(2, 7)	// 9
    
    function add(num1, num2) {
        return num1 + num2
    }
    ```

  - **반면, 함수 표현식으로 선언한 함수는 함수 정의 전에 호출 시 에러 발생**

  - 함수 표현식으로 정의된 함수는 변수로 평가되어 변수의 scope 규칙을 따름

    ```javascript
    sub(7, 2)	// Uncaught ReferenceError: Cannot access 'sub' before initializtion
    
    const sub = function (num1, num2) {
        return num1 - num2
    }
    ```

  - 함수 표현식을 var 키워드로 작성한 경우, 변수가 선언전 undefined로 초기화 되어 다른 에러가 발생

    ```javascript
    console.log(sub) // undefined
    sub(7, 2) // Uncaught TypeError: sub is not a function
    
    var sub = function (num1, num2) {
        return num1 - num2
    }
    ```

    



### Arrow Function

---

화살표 함수(Arrow Function)

- 함수를 비교적 간결하게 정의할 수 있는 문법

- function 키워드 생략 가능

- 함수의 매개변수가 단 하나 뿐이라면 `()`도 생략 가능

- 함수 몸통이 표현식 하나라면 `{}`과 return도 생략 가능

- 기존 function 키워드 사용 방식과의 차이점은 후반부 this 키워드를 학습하고 다시 설명

  ```javascript
  const arrow1 = function (name) {
    return 'hello'
  }
  
  // 1. function 키워드 삭제
  const arrow1 = (name) => { return 'hello' }
  
  // 2. 매개변수가 1개일 경우에는 ( ) 생략 가능
  const arrow1 = name => {return 'hello'}
  
  // 3. 함수 몽통이 return을 포함한 표식 1개일 경우 {} & return 을 삭제 
  const arrow1 = name => 'hello'
  ```



## 문자열 (String)

### 문자열 관련 주요 메서드 목록

| 메서드       | 설명                                       | 비고                                               |
| ------------ | ------------------------------------------ | -------------------------------------------------- |
| **includes** | 특정 문자열의 존재 여부를 참/거짓으로 반환 |                                                    |
| **split**    | 문자열을 토큰 기준으로 나눈 배열 반환      | 인자가 없으면 기존 문자열을 <br />배열에 담아 반환 |
| **replace**  | 해당 문자열을 대상 문자열로 교체하여 반환  | replaceAll                                         |
| **trim**     | 문자열의 좌우 공백 제거하여 반환           | trimStart, trimEnd                                 |



#### includes

- `string,includes(value)`

  - 문자열에 value가 존재하는지 판별 후 참 또는 거짓 반환

  ```javascript
  const str = 'a santa at nasa'
  
  str.includes('santa')	// true
  str.includes('asan')	// false
  ```

  

#### split

- `string.split(value)`

  - value가 없을 경우, 기존 문자열을 배열에 담아 반환
  - value가 빈 문자열일 경우 각 문자로 나눈 배열을 반환
  - value가 기타 문자열일 경우, 해당 문자열로 나눈 배열을 반환

  ```javascript
  const str = 'a cup'
  
  str.split()		// ['a cup']
  str.split('')	// ['a', ' ', 'c', 'u', 'p']
  str.split(' ')	// ['a', 'cup']
  ```



#### replace

- `string.replace(from, to)`

  - 문자열에 from 값이 존재할 경우, 1개만 to 값으로 교체하여 반환

- `string.replaceAll(from, to)`

  - 문자열에 from 값이 존재할 경우, 모두 to 값으로 교체하여 반환

  ```javascript
  const str = 'a b c d'
  
  str.replace(' ', '-')		// 'a-b c d'
  str.replaceAll(' ', '-')	// 'a-b-c-d'
  ```



#### trim

- `string.trim()`

  - 문자열 시작과 끝의 모든 공백문자(스페이스, 탭, 엔터 등)를 제거한 문자열 반환

- `string.trimStart()`

  - 문자열 시작의 공백문자(스페이스, 탭, 엔터 등)를 제거한 문자열 반환

- `string.trimEnd()`

  - 문자열 끝의 공백문자(스페이스, 탭, 엔터 등)을 제거한 문자열 반환

  ```javascript
  const str = '    hello    '
  
  str.trim()			// 'hello'
  str.trimStart()		// 'hello    '
  str.trimEnd()		// '    hello'
  ```

  



## 배열, 객체

### 배열 (Array)

---

#### 배열의 정의와 특징

- 키와 속성들을 담고 있는 참조 타입의 객체(object)

- 순서를 보장하는 특징이 있음

- 주로 대괄호를 이용하여 생성하고, 0을 포함한 양의 정수 인덱스로 특정 값에 접근 가능

- 배열의 길이는 array.length 형태로 접근 가능

  - [참고] 배열의 미자막 원소는 array.length - 1로 접근

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  
  console.log(numbers[0])		// 1
  console.log(numbers[-1])	// undefined
  console.log(numbers.length)	// 5
  ```

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  
  console.log(numbers[numbers.length - 1]) // 5
  console.log(numbers[numbers.length - 2]) // 4
  console.log(numbers[numbers.length - 3]) // 3
  console.log(numbers[numbers.length - 4]) // 2
  console.log(numbers[numbers.length - 5]) // 1
  ```

  

### 배열 관련 주요 메서드 목록(1) - 기본편

---

|       메서드        |                       설명                       |           비고           |
| :-----------------: | :----------------------------------------------: | :----------------------: |
|     **reverse**     |     원본 배열의 요소들의 순서를 반대로 정렬      |                          |
|   **push & pop**    |      배열의 가장 뒤에 요소를 추가 또는 제거      |                          |
| **unshift & shift** |      배열의 가장 앞에 요소를 추가 또는 제거      |                          |
|    **includes**     | 배열에 특정 값이 존재하는지 판별 후 참/거짓 반환 |                          |
|     **indexOf**     | 배열에 특정 값이 존재하는지 판별 후 인덱스 반환  | 요소가 없을 경우 -1 반환 |
|      **join**       |    배열의 모든 요소를 구분자를 이용하여 연결     | 구분자 생략 시 쉼표 기준 |



#### reverse

- `array.reverse()`

  - 원본 배열의 요소들의 순서를 반대로 정렬

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  numbers.reverse()
  console.log(numbers)	// [5, 4, 3, 2, 1]
  ```

  

#### push & pop

- `array.push()`

  - 배열의 가장 뒤에 요소 추가

- `array.pop()`

  - 배열 마지막 요소 제거

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  
  numbers.push(100)
  console.log(numbers)	// [1, 2, 3, 4, 5, 100]
  
  numbers.pop()
  console.log(numbers)	// [1, 2, 3, 4, 5]
  ```

  

#### unshift & shift

- `array.unshift()`

  - 배열의 가장 앞에 요소 추가

- `array.shift()`

  - 배열의 첫번째 요소 제거

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  
  numbers.unshift(100)
  console.log(numbers)	// [100, 1, 2, 3, 4, 5]
  
  numbers.shift()
  console.log(nubmers)	// [1, 2, 3, 4, 5]
  ```

  

#### includes

- `array.includes(value)`

  - 배열에 특정 값이 존재하는지 판별 후 참 또는 거짓 반환

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  
  console.log(numbers.includes(1))	// true
  
  console.log(numbers.includes(100))	// false
  ```

  

#### indexOf

- `array.indexOf(value)`

  - 배열의 특정 값이 존재하는지 확인 후 가장 첫 번째로 찾은 요소의 인덱스 반환
  - 만약 해당 값이 없을 경우 -1 반환

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  let result
  
  result = nubmers.indexOf(3)	// 2
  console.log(result)
  
  result = nubmers.indexOf(100) // -1
  console.log(result)
  ```

  

#### join

- `array.join([separator])`

  - 배열의 모든 요소를 연결하여 반환
  - separator(구분자)는 선택적으로 지정 가능하며, 생략 시 쉼표를 기본 값으로 사용

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  let result
  
  result = numbers.join()		// 1,2,3,4,5
  console.log(result)
  
  result = numbers.join('')	// 12345
  console.log(result)
  
  result = numbers.join(' ')	// 1 2 3 4 5
  console.log(result)
  
  result = numbers.join('-')	// 1-2-3-4-5
  console.log(result)
  ```



#### Spread Operator

- spread operator(...)를 사용하면 배열 내부에서 배열 전개 가능

- ES5까지는 Array.concat() 메서드를 사용

- 얕은 복사에 활용 가능

  ```javascript
  const array = [1,2,3]
  const newArray = [0, ...array, 4]
  
  console.log(newArray)	// [0, 1, 2, 3, 4]
  ```

  

### 배열 관련 주요 메서드 목록(2) - 심화편

---

- 배열을 순회하며 특정 로직을 수행하는 메서드
- 메서드 호출 시 인자로 callback 함수를 받는 것이 특징
  - callback 함수: 어떤 함수의 내부에서 실행될 목적으로 인자로 넘겨받는 함수를 말함

| 메서드  |                             설명                             |     비고     |
| :-----: | :----------------------------------------------------------: | :----------: |
| forEach |        배열의 각 요소에 대해 콜백 함수를 한 번씩 실행        | 반환 값 없음 |
|   map   |      콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환      |              |
| filter  | 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환 |              |
| reduce  |    콜백 함수의 반환 값들을 하나의 값(acc)에 누적 후 반환     |              |
|  find   |        콜백 함수의 반환 값이 참이면 해당 요소를 반환         |              |
|  some   |    배열의 요소 중 하나라도 판별 함수를 통과하면 참을 반환    |              |
|  every  |      배열의 모든 요소가 판별 함수를 통과하면 참을 반환       |              |



#### forEach

- `array.forEach(callback(element[, index[,array]]))`

  - 배열의 각 요소에 대해 콜백 함수를 한 번 씩 실행
  - 콜백 함수는 3가지 매개변수로 구성
    - element: 배열의 요소
    - index: 배열 요소의 인덱스
    - array: 배열 자체
  - 반환 값이 없는 메서드

  ```javascript
  array.forEach((element, index, array) = > {
      // do something
  })
  ```

  ```javascript
  const fruits = ['딸기', '수박', '사과', '체리'] 
  
  fruits.forEach((fruit, index) => {
      console.log(fruit, index)
  })
  
  // 딸기 0
  // 수박 1
  // 사과 2
  // 체리 3
  ```

  - [참고] 배열 순회 방법 비교

    | 방식       | 특징                                                         | 비고                         |
    | ---------- | ------------------------------------------------------------ | ---------------------------- |
    | for loop   | - 모든 브라우저 환경에서 지원<br />- 인덱스를 활용하여 배열의 요소에 접근<br />- break, continue 사용 가능 |                              |
    | for ... of | - 일부 오래된 브라우저 환경에서 지원<br />- 인덱스 없이 배열의 요소에 바로 접근 가능<br />- break, continue 사용 가능 |                              |
    | forEach    | - 대부분의 브라우저 환경에서 지원<br />- break, continue 사용 불가능 | Airbnb Style Guide 권장 방식 |

    

#### map

- `array.map(callback(element[, index[, array]]))`

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수 반환 값을 요소로 하는 새로운 배열 반환
  - 기존 배열 전체를 다른 형태로 바꿀 때 유용

  ```javascript
  array.map((element, index, array) => {
      // do something
  })
  ```

  ```javascript
  const numbers = [1, 2, 3, 4, 5]
  
  const doubleNums = numbers.map((num) => {
      return num * 2
  })
  
  console.log(doubleNums)	// [2, 4, 6, 8, 10]
  ```

  

#### filter

- `array.filter(callback(element[, index[, array]]))`

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환
  - 기존 배열의 요소들을 필터링할 때 유용

  ```javascript
  arraa.filter((element, index, array) => {
      //do something
  })
  ```

  ```javascript
  const nubmers = [1, 2, 3, 4, 5]
  
  const oddNums = numbers.filter((num, index) => {
      return num % 2
  })
  
  console.lgo(oddNums)	// 1, 3, 5
  ```

  

#### reduce

- `array.reduce(callbak(acc, element, [index[, array]])[, initialValue])`

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 반환 값들을 한나의 값(acc)에 누적 후 반환
  - reduce 메서드의 주요 매개변수
    - acc
      - 이전 callback 함수의 반환 값이 누적되는 변수
    - initialValue(optional)
      - 최초 callback 함수 호출 시 acc에 할당되는 값, default 값은 배열의 첫 번째 값
  - [참고] 빈 배열의 경우 initialValue를 제공하지 않으면 에러 발생

  ```javascript
  array.reduce((acc, element, index, array) => {
      // do something
  }, initialValue)
  ```

  ```javascript
  const numbers = [1, 2, 3]
  
  const result = numbers.reduce((acc, num) => {
      return acc + num
  }, 0)
  
  console.log(result) // 6
  
  // 동작 방식
  // 첫번째 동작 - acc: 0, num: 1
  // 두번째 동작 - acc: 1, num: 2
  // 세번째 동작 - acc: 3, num: 3
  ```

  

#### find

- `array.find(callback(element[,index[,array]]))`

  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 반환 값이 참이면, 조건을 만족하는 첫번째 요소를 반환
  - 찾는 값이 배열에 없으면 undefined 반환

  ```javascript
  array.find(element, index, arrray)) {
      // do something
  }
  ```

  ```javascript
  const averngers = [
      { name: 'Tony Stark', age: 45 },
      { name: 'Steve Rogers', age: 32 },
      { name: 'Thor', age: 40 },
  ]
  
  const result = avengers.find((avenger) => {
      return avenger.name === 'Tony Stark'
  })
  
  console.log(result)	// {name:'Tony Stark', age: 45}
  ```

  

#### some

- `array.some(callback(element[, index[, array]]))`

  - 배열의 요소중 하나라도 주어진 판별 함수를 통과하면 참을 반환
  - 모든 요소가 통과하지 못하면 거짓 반환
  - [참고] 빈 배열은 항상 거짓 반환

  ```javascript
  array.some((element, index, array) => {
      // do something
  })
  ```

  ```javascript
  const numbers = [1, 3, 5, 7, 9]
  
  const hasEvenNumber = numbers.some((num) => {
      return num % 2 === 0
  })
  console.log(hasEvenNumber)	// false
  
  const hasOddNumber = numbers.some((num) => {
      return num % 2
  })
  console.log(hasOddNumber)	// true
  ```

  

#### every

- `array.every(callback(element[, index[, array]]))`

  - 배열의 모든 요소가 주어진 판별 함수를 통과하면 참을 반환
  - 하나의 요소라도 통과하지 못하면 거짓 반환
  - [참고] 빈 배열을 항상 참 반환

  ```javascript
  arrary.every((element, index, array) => {
      // do something
  })
  ```

  ```javascript
  const numbers = [2, 4, 6, 8, 10]
  
  const isEveryNumberEven = numbers.every((num) => {
      return num % 2 === 0
  })
  console.log(isEveryNumberEven)	// true
  
  const isEveryNumberOdd = numbers.every((num) => {
      return num % 2
  })
  console.log(isEveryNumberOdd)	// false
  ```

  



### 객체

---

#### 객체의 정의와 특징

- 객체는 속성(property)의 집합이며, 중괄호 내부에 key와 value의 쌍으로 표현

- key는 문자열 타입만 가능

  - [참고] key 이름에 띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현

- value는 모든 타임(함수포함) 가능

- 객체 요소 접근은 점 또는 대괄호로 가능

  - [참고] key 이름에 띄어쓰기 같은 구분자가 있으면 대괄호 접근만 가능

  ```javascript
  const me = {
      name: 'jack',
      phoneNumber: '01012345678',
      'samsung products': {
          buds: 'Galaxy Buds Pro',
          galaxy: 'Galaxy s20',
      },
  }
  
  console.log(me.name)
  console.log(me.phoneNumber)
  console.log(me['samsung products'])
  console.log(me['samsung products'].buds)
  ```



#### 객체와 메서드

- 메서드는 객체의 속성이 참조하는 함수

- 객체, 메서드명()으로 호출 가능

- 메서드 내부에서는 this 키워드가 객체를 의미함

  - fullName은 메서드가 아니기 때문에 정상출력 되지 않음(NaN)
  - getFullName은 메서드이기 때문에 해당 객체의 firstName과 lastName을 정상적으로 이어서 반환

  ```javascript
  const me = {
      firstName: 'John',
      lastName: 'Doe',
      
      fullName: this.firstName + this.lastName,
      
      getFullName: function () {
          return this.firstName + this.lastName
      }
  }
  ```

  

#### 객체 관련 ES6 문법

**속성명 축약**

- 객체를 정의할 때 key와 할당하는 변수의 이름이 같으면 예시와 같이 축약 가능

  ```javascript
  const books = ['어쩌구', '저쩌구']
  const magazines = ['오쪼구', '조쪼구']
  
  const bookShop = {
      books,
      magazines,
  }
  console.log(bookShop)
  ```

**메서드명 축약**

- 메서드 선언시 function 키워드 생략 가능

  ```javascript
  const obj = {
      greeting() {
          console.log('Hi!')
      }
  }
  
  obj.greeting()	// Hi!
  ```

**계산된 속성 (computed property name)**

- 객체를 정의할 때 key의 이름을 표현식을 이용하여 동적으로 생성 가능

  ```javascript
  const key = 'regions'
  const value = ['광주', '대전', '구미', '서울']
  
  const ssafy = {
      [key]: value,
  }
  
  console.log(ssafy)			// { regions: Array(4) }
  console.log(ssafy.regions)	// ['광주', '대전', '구미', '서울']
  ```

**구조 분해 할당 (destructing assignment)**

- 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

  ```javascript
  const userInformation = {
      name: 'ssafy kim',
      userId: 'ssafyStudent1234'
      phoneNumber: '010-1234-5678'
      email: 'ssafy@ssafy.com'
  }
  
  const name = userInformation.name
  const userId = userInformation.userId
  const phtoneNumber = userInformation.phoneNumber
  const email = userInformation.email
  
  // 구조분해 할당
  const { name } = userInformation
  const { userId } = userInformation
  const { phoneNubmer} = userInformation
  const { email } = userInformation
  
  // 여러개도 가능
  const { name, userId } = userInformation
  ```

**Spread operator**

- spread operator(...)를 사용하면 객체 내부에서 객체 전개 가능

- 얕은 복사에 활용 가능

  ```javascript
  const obj = {b: 2, c: 3, d: 4}
  const newObj = {a: 1, ...obj, e: 5}
  
  console.log(newObj)	// {a: 1, b:2, c: 3, d: 4, e: 5}
  ```



#### JSON (JavaScript Object Notation)

- key - value 쌍의 형태로 데이터를 표기하는 언어 독립적 표준 포맷

- 자바스크립트의 객체와 유사하게 생겼으나 실제로는 문자열 타입

  - 따라서 JS의 객체로써 조작하기 위해서는 구문 분석(Parsing)이 필수

- 자바스크립트에서는 JSON을 조작하기 위한 두 가지 내장 메서드를 제공

  - JSON.parse()
    - JSON => 자바스크립트 객체
  - JSON.stringify()
    - 자바스크립트 객체 => JSON

  ```javascript
  // Object => JSON
  
  const jsonData = JSON.stringify({
      coffee: 'Americano',
      iceCream: 'Cookie and cream',
  })
  
  console.log(jsonData)			// "{"coffee":"Americano", ...}"
  console.log(typeof jsonData)	// string
  ```

  ```javascript
  // JSON => Object
  
  const jsonData = JSON.stringify({
      coffee: 'Ameriacno',
      iceCream: 'Cookie and cream',
  })
  
  const parseData = JSON.parse(jsonData)
  
  console.log(parseData)			// {coffee: 'Americano', ...}
  console.log(tyepof parseData)	// object
  ```

  - [참고] 배열은 객체다
    - 키와 속성들을 담고 있는 참조 타입의 객체
    - 배열은 인덱스를 키로 가지며 length 프로퍼티를 갖는 특수한 객체



## this

- JS의 this는 실행 문맥에 따라 다른 대상을 가리킨다.
- class 내부의 생성자(constructor) 함수
  - this는 생성되는 객체를 가리킴(Python의 self)
- 메서드(객체.메서드명()으로 호출 가능한 함수)
  - this는 해당 메서드가 소속된 객체를 가리킴
- 위의 두가지 경우를 제외하면 모두 최상위 객체(window)를 가리킴
- 자세한 것은 관련 pdf 참고!



## lodash

- 모듈성, 성능 및 추가 기능을 제공하는 Javascript 유틸리티 라이브러리

- array, object 등 자료구조를 다룰 때 사용하는 유용하고 간편한 유틸리티 함수들을 제공

- 함수 예시

  - reverse. srotBy, range, random, cloneDeep, ...

  ```html
  <body>
      <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
      <script>
          _.sample([1,2,3,4])
          _.sampleSize([1,2,3,4,],2)
          _.reverse([1,2,3,4])
          _.range(5)	
          _.range(1,5)
          _.range(1,5,2)
      </script>
  </body>
  ```

  ```html
  <body>
      <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
      <script>
          const original = { a: {b: 1} }
          const ref = original
          const copy = _.cloneDeep(original)
          
          console.log(original.a.b, ref.a.b, copy.a.b)
          ref.a.b = 10
          console.log(original.a.b, ref.a.b, copy.a.b)
          copy.a.b = 100
          console.log(original.a.b, ref.a.b, copy.a.b)
      </script>
  </body>
  ```

  - ladash를 사용하지 않을 경우 깊은 복자는 직접 함수를 만들어서 구현해야 한다. 내장된 깊은 복사 관련 함수가 없기 때문이다.

