# DB: SQL & ORM 

## Database

**데이터베이스 (DB)**

- 데이터베이스는 **체계화된 데이터의 모임**
- 여러 사람이 공유하고 사용할 목적으로 통합 관리되는 정보의 집합
- 논리적으로 연관된 (하나 이상의) 자료의 모음으로 그 내용을 고도로 구조화 함으로써 검색과 갱신의 효율화를 꾀한 것
- 즉, 몇 개의 자료 파일을 조직적으로 통합하여 자료항목의 중복을 없애고 자료를 구조화하여 기억 시켜 놓은 자료의 집합체



- 데이터베이스로 얻는 장점들
  - 데이터 중복 최소화
  - 데이터 무결성 (정확한 정보)
  - 데이터 일관성
  - 데이터 독립성
  - 데이터 표준화
  - 데이터 보안유지



**RDB** (관계형 데이터베이스)

- Realtional Database
- **키와 값들의 간단한 관계를 표 형태로 정리한 데이터베이스**
- 관계형 모델에 기반



- **관계형 데이터베이스 용어**

  - **스키마(**schema): 데이터베이스에서 자료의 구조, 표현방법, 관계 등 전반적인 명세를 기술한 것
  - **테이블**(table): 열(컬럼/ 필드)과 행(레코드/값)의 모델을 사용해 조직된 데이터 요소들의 집합
  - **열**(column): 각 열에는 고유한 데이터 형식이 지정됨
  - **행**(row): 실제 데이터가 저장되는 형태
  - **기본키**(Primary Key): 각 행(레코드)의 고유값
    - 반드시 설정해야 하며, 데이터베이스 관리 및 관계설정 시 주요하게 활용 됨 

  

**RBDMS** (관계형 데이터베이스 관리 시스템)

- Relational DataBase Management System
- 관계형 모델을 기반으로 하는 데이터베이스 관리시스템을 의미
- 예) MySQL, SQLite, PpstgreSQL, ORACLE, MS SQL



- **SQLite**
  - 서버 형태가 아닌 파일 형식으로 응용프로그램에 넣어서 사용하는 비교적 가벼운 데이터베이스 
  - 구글 안드로이드 운영체제에 기본적으로 탑재된 데이터베이스이며, 임베디드 소프트웨어에도 많이 활용됨



- **Sqlite Data Type (동적)**
  1.  **NULL** (=NONE)
  2.  **INTEGER**: 정수 
  3.  **REAL**: 부동 소수점 값
  4.  **TEXT**
  5.  **BLOB**: 입력된 그대로 정확히 저장된 데이터 (별다른 타입이 없이 그대로 저장)



- **Sqlite Type Affinity** (데이터타입 선호도)

  - **Type Affinity**
    - 특정 컬럼에 저장하도록 권장하는 데이터 타입

  1. **INTEGER**
  2. **TEXT**
  3. **BLOB**
  4. REAL
  5. **NUMERIC**



## SQL

**SQL** (Structured Query Language)

- 관계형 데이터베이스 관리시스템의 데이터 관리를 위해 설계된 특수목적의 프로그래밍 언어

- 데이터베이스 스키마 생성 및 수정
- 자료의 검색 및 관리
- 데이터베이스 객체 접근 조정 관리



- **분류**
  - **DDL** (data defintion language): 데이터 정의 언어
  - **DML** (data manipulation language): 데이터를 저장, 조회, 수정, 삭제 등을 하기 위한 명령어
  - **DCL** (data control language): 데이터베이스 사용자의 권한 제어를 위해 사용하는 명령어



- **SQL Keywords - Data Manipulation Language**
  - **INSERT**: 새로운 데이터 삽입(추가)
  - **SELECT**: 저장되어있는 데이터 조회
  - **UPDATE**: 저장되어있는 데이터 갱신
  - **DELETE**: 저장되어있는 데이터 삭제



**테이블 생성 및 삭제**

- **데이터베이스 생성하기**

  ```bash
  $ sqlite3 tutorial.sqlite3
  ```

  ```sqlite
  sqlite> .database
  ```

  

- **csv 파일을 table로 만들기**

  ```sqlite
  sqlite> .mode csv
  sqlite> .import hellodb.csv examples
  sqlite> .tables
  examples
  ```



- **SELECT**

  ```sqlite
  SELECT * FROM examples;
  ```

  - ;까지를 하나의 명령으로 간주



- **(Optional) 터미널 view 변경하기**

  ```sqlite
  sqlite> .headers on
  sqlite> .mode column
  sqlite>
  
  ```

  

- 진행 TIP - sqlite 확장프로그램 사용하기

  - `tutorial.sqlite3` 에서 `open database`
  - 하단의 `SQLITE EXPLORER`에서 `NEW QUERY`선택



- **특정 테이블의 schema 조회**

  ```sqlite
  sqlite> .schema classmates
  CREATE TABLE classmates (
  id INTEGER PRIMARY KEY,
  name TEXT
  )
  ```

  

- **DROP: 테이블 삭제**

  ```sqlite
  DROP TABLE classmates
  ```

  

### **CRUD** (Create/Read/Update/Delete)

---

- **CREATE**

  - **INSERT**

    - 'insert a single row into a table'
    - **테이블에  단일 행 삽입**

    ```sql
    INSERT INTO 테이블이름 (컬럼1, 컬럼2, ...) VALUES (값1, 값2, ...);
    ```

    - INSERT는 특정 테이블에 레코드를 삽입
    - 여러 값들을 한번에 자성하는 것도 가능(`,`로 구분해서 나열하면 된다.)

    - **새로운 열 삽입**

    ```sql
    INSERT INTO 테이블이름 VALUES (값1, 값2, ...);
    ```

    - **생성된 테이블 조회**

    ```sql
    SELECT * FROM classmates;
    ```

    - **id 포함하여 테이블 조회**

    ```sql
    SELECT rowid, * FROM classmates;
    ```

  - **NULL**

    - 꼭 필요한 정보라면 공백으로 비워두면 안된다.
    - **처음에 테이블 생성할 때 스키마에 `NOT NULL` 설정 필요!!!**

    ```sql
    CREATE TABLE classmates (
      id INTEGER PRIMARY KEY,
      name TEXT NOT NULL,
      age INT NOT NULL,
      address TEXT NOT NULL
    );
    ```

    - 주의! `PRIMARY KEY`를 생성할 때는 `INT`가 아닌 `INTEGER`로 해야만 한다.

  - **CREATE 실패**

    - 스키마에 id를 직접 작성했기 때문에 입력한 column들을 명시하지 않으면 자동으로 입력되지 않는다.
    - **해결 방법**

    1. **id를 포함한 모든 value를 작성**
    2. **각 value에 맞는 column들을 명시적으로 작성**



- **READ**

  - **LIMIT**

    - 'constrain the number of rows returned by a query'
    - 쿼리에서 반환되는 행 수를 제한
    - 특정 행부터 시작해서 조회하기 위해 OFFSET 키워드와 함께 사용하기도 함

  - **WHERE**

    - 'sqecify the search condition for rows returned by the query'
    - 쿼리에서 반환된 행에 대한 특정 검색 조건을 지정

  - **SELECT DISTINCT**

    - 'remove duplicate rows in the result set'

    - 조회 결과에서 중복 행을 제거

    - DISTINCT 절은 SELECT 키워드 바로 뒤에 작성해야 함

    - **SELECT statement**

      1. **모든 컬럼 조회하기**

      ```sql
      SELECT * FROM 테이블이름;	
      ```

      2.  **모든 컬럼 값이 아닌 특정 컬럼만 조회하기**

      ```sql
      SELECT 컬럼1, 컬럼2, ... FROM 테이블이름;	
      ```

    - **LIMIT**

      1. **원하는 수만큼 데이터 조회하기**

      ```sql
      SELECT 컬럼1, 컬럼2, ... FROM 테이블이름 LIMIT 숫자;
      ```

      2. **원하는 수만큼 데이터 조회하되 중간에 건너뛰기**

      ```sql
      SELECT 컬럼1, 컬럼2, ... FROM 테이블이름 LIMIT 숫자 OFFSET 숫자;
      ```

    - [참고] **OFFSET**

      - 동일 오브젝트 안에서 오브젝트 처음부터 주어진 요소나 지점까지의 변위차(위치변화량)을 나타내는 정수형

    - **WHERE**

      1. **특정 데이터(조건) 조회하기**

      ```sql
      SELECT 컬럼1, 컬럼2, ... FROM 테이블이름 WHERE 조건;
      ```

    - **DISTINCT**

      1. **특정 컬럼을 기준으로 중복없이 가져오기**

      ```sql
      SELECT DISTINCT 컬럼 FROM 테이블이름;
      ```

      

- **DELETE**

  - **DELETE statement**

    - **DELETE**

      - 'remove rows from a table'
      - 테이블에서 행을 제거
      - **조건을 통해 특정 레코드 삭제하기**

      ```sql
      DELETE FROM 테이블이름 WHERE 조건;
      ```

      -  **중복 불가능(UNIQUE)값인 rowid를 기준으로 삭제!**

    - **SQlite는 기본적으로 삭제된 id 값을 재활용한다.**

      - django와는 다른 점!

      - **AUTOINCREMENT**

        - Column attribute
        - 테이블을 생성하는 단계에서 설정 가능
        - **SQLite 가 사용되지 않은 값이나 이전에 삭제된 행의 값을 재사용하는 것을 방지**

        ```sqlite
        CREATE TABLE 테이블이름 (
        id INTEGER PRIMARY KEY AUTOINCREMENT, ...);
        ```

        

- **UPDATE**

  - **UPDATE statement**

    - **UPDATE**

      - 'update dat of existing rows in the table'
      - 기존 행의 데이터를 수정
      - SET clause 에서 테이블의 각 열에 대해 새로운 값을 설정
      - **조건을 통해 특정 레코드 수정하기**

      ```sqlite
      UPDATE 테이블이름 SET 컬럼1=값1, 컬럼2=값2, ... WHERE 조건;
      ```

      - **중복 불가능(UNIQUE)한 값인 rowid를 기준을 수정!**



### **WHERE**

---

- **Table users 생성**

  ```sqlite
  CREATE TABLE users (
  first_name TEXT NOT NULL,
  last_name TEXT NOT NULL,
  age INTEGER NOT NULL,
  country TEXT NOT NULL,
  phone TEXT NOT NULL,
  balance INTEGER NOT NULL
  );
  ```

- **csv 파일 정보를 테이블에 적용하기**

  ```sqlite
  sqlite> .mode csv
  sqlite> .import users.csv users
  sqlite> .tables
  classmates examples users
  ```

- **WHERE 활용**

  -  users 테이블에서 age가 30 이상인 유저의 모든 컬럼 정보를 조회하려면?

    ```sqlite
    SELECT * FROM users WHERE age >= 30;
    ```

  - users 테이블에서 age가 30 이상인 유저의 first_name 컬럼 정보를 조회하려면?

    ```sqlite
    SELECT first_name FROM users WHERE age >= 30;
    ```

  - users 테이블에서 age가 30 이상이고 성이 '김'인 사람의 나이와 성만 조회하려면?

    ```sqlite
    SELECT age, first_name FROM users WHERE age >= 30 AND last_name='김';
    ```



### **Aggregate Functions**

---

- **집계함수**
- **값 집합에 대한 계산을 수행하고 단일 값을 반환**
  - 여러행으로부터 하나의 결괏값을 반환하는 함수
- **SELECT 구문에서만 사용**된다.



- **종류**
  - **COUNT**: 그룹의 항목 수를 가져옴
  - **AVG**: 모든 값의 평균을 계산
  - **MAX**: 그룹에 있는 모든 값의 최대값을 가져옴
  - **MIN**: 그룹에 있는 모든 값의 최소값을 가져옴
  - **SUM**: 모든 값의 합을 계산



- **COUNT**

  - **레코드의 개수 조회하기**

  ```sqlite
  SELECT COUNT(컬럼) FROM 테이블이름;
  ```

  - users 테이블의 레코드 총 개수를 조회한다면?

  ```sqlite
  SELECT COUNT(*) FROM users;
  ```

  

- **AVG, MAX, MIN, SUM**

  - **레코드의 평균, 최대값, 최소값, 합 구하기 (INTEGER에만 사용 가능)**

  ```sqlite
  SELECT AVG(컬럼) FROM 테이블이름;
  SELECT MAX(컬럼) FROM 테이블이름;
  SELECT MIN(컬럼) FROM 테이블이름;
  SELECT SUM(컬럼) FROM 테이블이름;
  ```

  - 30살 이상인 사람들의 평균 나이는?

  ```sqlite
  SELECT AVG(age) FROM users WHERE age >= 30;
  ```

  - 계좌 잔액이 가장 높은 사람과 그 액수를 조회하려면?

  ```sqlite
  SELECT first_name, MAX(balacne) FROM users;
  ```



### **LIKE**

---

- **LIKE operator**
  - 'query data based on pattern matching'
  - **패턴 일치를 기반으로 데이터를 조회**하는 방법
  - **SQLite는 패턴구성을 위한 2개의 wildcards를 제공**
    - **% (percent sign)**
      - 0개 이상의 문자
    - **`-` (underscore)**
      - 임의의 단일 문자

- **[참고] wildcard character**
  - 파일을 지정할 때 구체적인 이름 대신에 여러 파일을 동시에 지정할 목적으로 사용하는 특수 기호
    - `*, ?`등
  - 주로 특정한 패턴이 있는 문자열 혹은 파일을 찾거나 긴 이름을 생략할 때 쓰임
  - 텍스트 값에서 알 수 없는 문자를 사용할 수 있는 특수 문자로, 유사하지만 동일한 데이터가 아닌 여러 항목을 찾기에 매우 편리한 문자
  - 지정된 패턴 일치를 기반으로 데이터를 수집하는 데도 도움이 될 수 있음



- **LIKE statment**

  - **패턴을 확인하여 해당하는 값을 조회하기**

  ```sqlite
  SELECT * FROM 테이블 WHERE 컬럼 LIKE '와일드카드패턴';
  ```

  - **wildcards의 2가지 패턴**
    1. `%` (percent sign): 이 자리에 문자열이 있을 수도, 없을 수도 있다.
    2. `_` (underscore): 반드시 이 자리에 한 개의 문자가 존재해야 한다.
  - **wildcards 사용예시**
    - `2%`: 2로 시작하는 값
    - `%2`: 2로 끝나는 값
    - `%2%`: 2가 들어가는 값
    - `_2%`: 아무 값이 하나 있고 두 번째가 2로 시작하는 값
    - `1___`: 1로 시작하고 총 4자리인 값
    - `2_%_%` / `2__%`: 2로 시작하고 적어도 3자리인 값

  

  - users 테이블에서 나이가 20대인 사람만 조회한다면?

  ```sqlite
  SELECT * FROM users WHERE age LIKE '2_';
  ```

  - users 테이블에서 지역 번호가 02인 사람만 조회한다면?

  ```sqlite
  SELECT * FROM users WHERE phone LIKE '02-%';
  ```

  - users 테이블에서 이름이 '준'으로 끝나는 사람만 조회한다면?

  ```sqlite
  SELECT * FROM users WHERE first_name LIKE '%준';
  ```

  - users 테이블에서 중간 번호가 5114인 사람만 조회한다면?

  ```sqlite
  SELECT * FROM users WHERE phone LIKE '%-5114-%';
  ```



### **ORDER BY & GROUP BY**

---

- **ORBER BY**

  - **ORDER BY clause**
    - 'sort a result set of a query'
    - 조회 결과 집합을 정렬
    - SELECT 문에 추가하여 사용
    - **정렬 순서를 위한 2개의 keyword 제공**
      - **ASC - 오름차순 (기본값)**
      - **DESC - 내림차순**

  

  - **특정 컬럼을 기준으로 데이터를 정렬해서 조회하기**
    - **정렬 기준이 여러개면 앞의 것을 우선으로 먼저 정렬한다.**

  ```sqlite
  SELECT * FROM 테이블 ORDER BY 컬럼 ASC;
  SELECT * FROM 테이블 ORDER BY 컬럼1, 컬럼2 DESC;
  ```

  - users에서 나이순으로 오름차순 정렬하여 상위 10개만 조회한다면?

  ```sqlite
  SELECT * FROM users ORDER BY age ASC LIMIT 10;
  ```

  - users에서 나이 순, 성 순으로 오름차순 정렬하여 상위 10개만 조회한다면?

  ```sqlite
  SELECT * FROM users ORDER BY age, last_name ASC LIMIT 10;
  ```

  - users에서 계좌 잔액 순으로 내림차순 정렬 하여 해당 유저의 성과 이름을 10개만 조회한다면?

  ```sqlite
  SELECT * FROM users ORDER BY balance DESC LIMIT 10;
  ```

  

- **GROUP BY**

  -  **GROUP BY clause**
    - 'make a set of summary rows from a set of rows'
    - 행 집합에서 요약 행 집합을 만듦 
    - SELECT 문의 optional 절
    - 선택된 행 그룹을 하나 이상의 열 값으로 요약 행으로 만듦
    - **주의: 문장에 WHERE 절이 포함된 경우 반드시 WHERE 절뒤에 작성해야 함!**

  

  - **지정된 기준에 따라 행 세트를 그룹으로 결합. 데이터를 요약하는 상황에 주로 사용**

  ```sqlite
  SELECT 컬럼1, aggregate_function(컬럼2) FROM 테이블 GROUP BY 컬럼1, 컬럼2;
  ```

  - users에서 각 성(last_name)씨가 몇 명씩 있는지 조회한다면?

  ```sqlite
  SELECT last_name, COUNT(*) FROM users GROUP BY last_name;
  ```

  - **AS를 활용해서 COUNT에 해당하는 컬럼명을 지정할 수 있음**

  ```sqlite
  SELECT last_name, COUNT(*) AS name_count FROM users GROUP BY last_name;
  ```

  

### **ALTER TABLE**

---

- **ALTER TABLE statement**

  - **ALTER TABLE의 3가지 기능**

  1. **table 이름 변경**
  2. **테이블에 새로운 column 추가**
  3. **[참고] column 이름 수정 (new in sqlite 3.25.0)**

  

  - **table 이름 변경하기**

  ```sqlite
  ALTER TABLE 기존테이블이름 RENAME TO 새로운테이블이름;
  ```

  - **테이블에 새로운 column 추가하기**

  ```sqlite
  ALTER TABLE 테이블이름 ADD COLUMN 칼럼이름 데이터타입설정;
  ```

  - column 추가 실패

    - 테이블에 있던 기존 레코드들에는 새로 추가할 필드에 대한 정보가 없다.
    - 그러므로 NOT NULL 형태의 컬럼은 추가가 불가능!

  - 해결방법 2가지

    1. NOT NULL 설정 없이 추가하기

       ```sqlite
       ALTER TABLE news ADD COLUMN created_at TEXT;
       ```

    2. 기본 값 설정하기

       ```sqlite
       ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT '소제목';
       ```

  - **[참고]columns 이름 수정하기**

  ```sqlite
  ALTER TABLE 테이블이름 RENAME COLUMN 기존컬럼이름 TO 새로운컬럼이름:
  ```

  - **[참고] column 삭제하기**

  ```sqlite
  ALTER TABLE 테이블이름 DROP COLUMN 컬럼이름;
  ```



---

### [참고사항]

---

- SQL과 ORM의 차이

  - 전체조회

    - SQL

    ```sql
    SELECT * FROM articles_article;
    ```

    - ORM

    ```python
    Articles.objects.all()
    ```



- IPython shell 에서 ORM이 SQL로 변환되는 과정 확인하기

  ```python
  print(Articles.objects.all())
  ```

  ```python
  print(Aritcles.objects.filter(pk=1).query)
  ```



---

### [주의사항]

---

- schema를 먼저 생성하고 csv를 import하는 것이 맞는 순서이다. 안그러면 전부 TEXT로 된다.
- 모든 명령어 문장 끝에 세미콜론을 더해야함
- 프라이머리 키는 INT 가 아니라 INTEGER로 속성을 입혀야만 함
- 공식문서 대신에 SQLITE TUTORIAL 참고
