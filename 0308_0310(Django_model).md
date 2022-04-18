# Django

---

### Model

---

- **Model**
  - 단일한 데이터에 대한 정보를 가짐
    - 사용자가 저장하는 데이터들의 필수적인 필드(열)들과 동작들을 포함
  - 저장된 데이터베이스의 구조
  - Django는 model을 통해 데이터에 접속하고 관리
  - 일반적으로 각각의 model은 하나의 데이터베이스 테이블에 매핑됨



- **Database (DB)**
  - **데이터 베이스**: 체계화된 데이터의 모임
  - **쿼리(Query)**
    - 데이터를 조회하기 위한 명령어
    - 조건에 맞는 데이터를 추출하거나 조작하는 명령어
    - 'Query를 날린다.' -> DB를 조작한다.
- **Database의 기본 구조**
  - **스키마 (Schema)**
    - 데이터베이스에서의 자료의 구조, 표현방법, 관계등을 정의한 구조
    - 데이터베이스의 구조와 제약조건에 관련한 전반적인 명세를 기술한 것
  - **테이블**
    - 열(필드)과 행(레코드)의 모델을 사용해 조직된 데이터 요소들의 집합
    - SQL 데이터베이스에서는 테이블을 관계라고도 한다.
    - **열(Column), 필드**: 각 열에는 고유한 데이터 형식이 지정된다.
      - INTEGER TEXT NULL 등
    - **행(ROW), 레코드**: 테이블의 데이터는 행에 저장된다.
  - **PK(기본키)**
    - 각 행(레코드)의 고유값으로 Primary Key로 불린다.
    - 반드시 설정해야 하며, 데이터베이스 관리 및 관계 설정시 주요하게 활용된다.
    - 데이터에 접근하기 위해 사용되는 값



- **Model 정리**
  - 웹 애플리케이션의 데이터를 구조화하고 조작하기 위한 도구
  - 모델을 통해서 DB를 구조화, 조작해서 데이터를 다룬다.



### ORM

---

- **ORM**
  - **Object-Relational-Mapping**
  - **객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간(Django - SQL) 데이터를 변환하는 프로그래밍 기술**
  - OOP 프로그래밍에서 RDBMS을 연동할 때, 데이터베이스와 객체지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법
  - DJango는 내장 DJango ORM을 사용함
- **ORM의 장점과 단점**
  - **장점**
    - SQL을 잘 알지 못해도 DB 조작이 가능
    - SQL의 절차적 접근이 아닌 객체 지향적 접근으로 인한 높은 생산성
  - **단점**
    - ORM 만으로 완전한 서비스를 구현하기 어려운 경우가 있음
  - 현대 웹 프레임워크의 요점은 웹 개발의 속도를 높이는 것
- ORM을 **사용하는 이유**
  - **DB를 객체로서 사용하기 위해서**



- **models.py작성**

  ```python
  # articles/models.py
  
  class Article(models.Model):
      title = models.CharField(max_length=10)
      content = models.TextField()
  ```

  - 테이블에 대한 스키마를 설정 (장고에서 PK값을 자동생성해서 3개의 컬럼이 생성된다.)
  - `TextField`와 `CharField`는 둘 다 문자열 필드이지만`CharField`는 max_length를 필수 인자로 가진다.
  - 각 모델은 django.models.Model 클래스의 서브 클래스로 표현됨
    - django.db.models 모듈의 Model 클래스를 상속받음
  - models 모듈을 통해 어떠한 타입의 DB 컬럼을 정의할 것인지 정의
    - title과 content은 모델의 필드를 나타냄
    - 각 필드는 클래스 속성으로 지정되어 있으며 각 속성은 각 데이터베이스의 열에 매핑(연결)
    - 데이터 타입이 뭔지 알려주는 것

- **사용 모델 필드**

  - **CharField(max_length=None, **options)**
    - 길이의 제한이 있는 문자열을 넣을 때 사용
    - CharField의 max_length는 필수 인자
    - 필드의 최대길이(문자), 데이터 베이스 레벨과 Django의 유효성 검사(값을 검증하는 것)에서 활용
  - **TextField(**options)**
    - 글자의 수가 많은 때 사용
    - max_length 옵션 작성시 자동 양식 필드인 textarea위젯에 반영은 되지만 모델과 데이터베이스 수준에는 적용되지 않음



### Migrations

---

- **Migrations**
  - Django가 model에 생긴 변화를 반영하는 방법
  - Migration 실행 및 DB 스키마를 다루기 위한 몇가지 명령어
    - **makemigrations (필수)**
    - **migrate (필수)**
    - sqlmigrate
    - showmigrations



- **Migrations Commands**

  1. **makemigrations**

     ```bash
     $ python manage.py makemigrations
     ```

     - model을 변경한 것에 기반한 새로운 마이그레이션(설계도)을 만들 때 사용
     - 즉 models.py에 변경사항이 생길 때마다 설계도를 만들어야 함

     - migrations 폴더 내의 마이그레이션 파일들 생성
     - models.py에 변동사항이 생겼어도 설계도 즉, 스키마에 직접적인 변동이 없으면 먹히지 않는다.

  2. **migrate**

     ```bash
     $ python manage.py migrate
     ```

     - 마이그레이션을 DB에 반영하기 위해 사용
     - 설계도를 실제 DB에 반여하는 과정
     - 모델에서의 변경사항들과 DB의 스키마가 동기화를 이룸
     - 생성된 sql 파일은 마우스 오른쪽 버튼 클릭 후에 '`open database`' 클릭, 맨 아래 `SQLITE EXPLORER` 누르면 생성된 데이터베이스 테이블을 볼 수 있다.

  3. **sqlmigrate**

     ```bash
     $ python manage.py sqlmigrate 앱이름 설계도번호
     ```

     - 마이그레이션에 대한 SQL구문을 보기 위해 사용
     - 마이그레이션이 SQL문으로 어떻게 해석되어서 동작할지 미리 확인할 수 있음

  4. **showmigrations**

     ```bash
     $ python manage.py showmigrations
     ```

     - 프로젝트 전체의 마이그레이션 상태를 확인하기 위해 사용
     - 마이그레이션 파일들이 migrate됐는지 안됐는지 여부를 확인 할 수 있음 (디버깅에 활용)
     - `[X]`: migrate가 잘 되어 있음을 의미



- **반드시 기억해야할 migration 3단계**
  1. **models.py**
     - models 변경사항 발생시
  2. **$ python manage.py makemigrations**
     - migrations 파일 생성
  3. **$ python manage.py migrate**
     - DB 반영 (모델과 DB의 동기화)



- **사용 모델 필드**
  - DateTimeField
    - DateField를 상속 받는 클래스
  - **DataField's options (이거 시험 나옴!!!)**
    - **auto_now_add**
      - **최초 생성일자**
      - Django ORM이 최초 insert 입력시에만 현재 날짜와 시간으로 갱신
    - **auto_now**
      - **최종 수정 일자**
      - Django ORM이 save를 할 때마다 현재 날짜와 시간으로 갱신



### Database API

---

- **DB API**

  - DB를 조작하기 위한 도구
  - Django가 기본적으로 ORM을 제공함에 따른 것으로 DB를 편하게 조작할 수 있도록 도움
  - Model을 만들면 Django는 객체들을 만들고 읽고 수정하고 지울 수 있는 database-abstract API를 자동으로 만듦

- **DB API 구문 - Making Queries**

  ```python
  Article.objects.all()
  ```

  - Class Name / Manager / QuerySet API



- **Manager**
  - Django 모델에 데이터베이스 query 작업이 제공되는 인터페이스
  - 기본적으로 모든 Django 모델 클래스에 objects라는 Manager를 추가 (고정되어있으므로 생각할 필요 없음)
- **QuerySet**
  - **데이터베이스로부터 전달받은 객체 목록**
  - queryset 안의 객체는 0개, 1개 혹은 여러 개일 수 있음
  - 데이터베이스로부터 조회, 필터, 정렬 등을 수행할 수 있음



- **Django shell**

  - 일반 Python shell을 통해서는 장고 프로젝트 환경에 접근할 수 없음

  - 그래서 장고 프로젝트 설정이 load된 Python shell을 활용해 DB API 구문 테스트 진행

  - 기본 Django shell 보다 더 많은 기능을 제공하는 **shell_plus를 사용해서 진행**

  - **python shell 환경 키기**

    ```bash
    $ python -i
    ```

  - **django shell 환경 키기**

    ```bash
    $ pip install ipython
    $ pip install django-extensions
    ```

    - ipython은 파이썬 환경을 좀더 강력하게 해줌
    - 설치후에는 settings.py의 INSTALLED_APPS에 'django_extensions' 등록

    ```bash
    $ python manage.py shell_plus
    ```

    - 환경 시작과 동시에 필요한 것들을 임포트 해줌



### CRUD

---

- **CRUD**
  - 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 생성(Create), 읽기(Read), 갱신(Update), 삭제(Delete)를 묶어서 일컫는 말



- **Create (생성)**

  - 생성에는 **3 가지 방법**이 존재

  1. **인스턴스 생성 후 인스턴스 변수 설정** (OOP다루는 것 처럼)

     (저장시간은 항상 뭘해도 UTC 시간으로 저장되기 때문에 걱정 안해도 됨)

     ```bash
     article = Article()
     article.title = 'first'
     article.content = 'django!'
     article.save()
     ```

  2. **초기값과 함께 인스턴스 생성**

     ```bash
     article = Article(title='second', content='django!!')
     article.save()
     ```

  3. **QuerySet API - create() 사용** - **바로 return값을 돌려줌(create함수 내에 save가 포함)**

     ```bash
     Article.objects.create(title='third', content='django!!!')
     ```

     

  - **CREATE 관련 메서드**
    - **save()** method
      - Saving objects
      - **객체를 데이터베이스에 저장**함
      - 데이터 생성 시 save()를 호출하기 전에는 객체의 ID 값이 무엇인지 알 수 없음
        - ID 값은 데이터 베이스에서 생성되는 것이기 때문이다.
      - 단순히 모델을 인스턴스화하는 것은 DB에 영향을 미치지 않기 때문에 save 필요!



- **Read (조회)**

  - QuerySet API method를 사용해 다양한 조회를 하는 것이 중요
  - QuerySet API method는 크게 2가지로 분류
    1. **Methods that return new querysets** (새로운 쿼리셋)
       - all(), filter() 등
    2. **Methods that do not return querysets** (쿼리셋 리턴을 하지 않음)
       - get(), create() 등

  

  - **Read 관련 메서드**

    - **all()**

      - 전체 오브젝트 조회

      ```bash
      Aritcle.objects.all()
      ```

    - **get()** - 파이썬 딕셔너리랑 아무 상관 없음

      - 주어진 lookup 매개변수와 일치하는 객체를 반환
      - 객체를 찾을 수 없으면 DoesNot Exist 에외를 발생시키고, 둘 이상의 객체를 찾으면 MultipleObjectsReturned 예외를 발생 시킴
      - 위와 같은 특징을 가지고 있기 때문에 primary key와 같이 고유성을 보장하는 조회에서 사용해야 함
      - 단일 객체가 반환

      ```bash
      article = Article.objects.get(pk=1)
      article
      ```

    - **filter()**

      - 주어진 lookup 매개변수와 일치하는 객체를 포함하는 새 QuerySet을 반환
      - get()과는 다르게 오류가 나지 않음
      - get()과는 다르게 쿼리셋이 반환

      ```bash
      article = Article.objects.filter(pk=1)
      ```



- **수정 & 삭제의 공통점: '무엇'을 수정하고 삭제를 할것인가? 즉 선조회가 필수이다.** (중요중요)



- **Update (수정)**

  - aritcle 인스턴스 객체의 인스턴스 변수의 값을 변경 후 저장

  - **조회하고 수정하고 반영**

    ```bash
    article = Ariticle.objects.get(pk=1)
    article.title = 'byebye'
    article.save()
    ```

    

- **Delete (삭제)**

  - **delete()**

    - QuerySet의 모든 행에 대해 SQL 삭제 쿼리를 수행한다.
    - 삭제된 객체 수와 객체 유형당 삭제 수가 포함된 딕셔너리를 반환한다.
    - 데이터베이스는 **삭제된 id 값을 재활용하지 않는다.**
      - create시간이 엉키는 문제도 있음
      - 문제가 있어서 삭제된 데이터는 재활용하지 않음

    ```bash
    article.delete()
    ```



- **Field lookups**

  - **조회시 특정 검색 조건을 지정** 

  - QuerySet 메서드 filter(), exclude() 및 get()에 대한 키워드 인수로 지정됨

  - 예시

    ```bash
    Article.objects.filter(pk__gt=2)
    ```



### CRUD with views

---

- **HTTP method**

  - **GET**
    - 특정 리소스를 가져오도록 요청할 때 사용
    - 반드시 데이터를 가져올 때만 사용해야 함 
    - DB에 변화를 주지 않음
    - CRUD에서 R 역할을 담당
    - 쿼리 스프링 파라미터이기 때문에 URL에 작성이 되어야한다.
    - '나 이것 좀 줘~'

  

  - **POST**
    - 서버로 데이터를 전송할 때 사용
    - 리소스를 생성, 변경하기 위해 데이터를 HTTP body에 담아 전송
    - POST의 return 함수는 render가 아닌 redirect이다
    - 서버에 변경사항을 만듦
    - CRUD 에서 CUD 역할을담당
    - '나 DB좀 변화시킬래~'
  
  
  
  - **사이트 간 요청 위조(Cross-site request forgery)**
    - 웹 애플리케이션 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 하거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법
    - Django는 CSRF에 대항하여 middleware와 template tag를 제공
    - CSRF라고도 함
  
  
  
  - **CSRF 공격 방어**
  
    - **Secruity Token 사용 방식 (CSRF Token)**
  
      - 사용자의 데이터에 임의의 난수 값을 부여해 매 요청마다 해당 난수 값을 포함시켜 전송시키도록함 
      - 이후 서버에서 요청을 받을때마다 전달된 token 값이 유효한지 검증
  
    - 일반적으로 데이터 변경이 가능한 POST, PATCH, DELETE Method 등에 적용 (GET 제외)
  
    - **Django는 CSRF token 템플릿 태그를 제공**
  
      ```html
      {% csrf_token %}
      ```
  
      ```html
      <input type="hidden" name="csrfmiddlewaretoken" value="난수값">
      ```
  
      - CSRF 보호에 사용
  
    - **CsrfViewMiddleware**
  
      - CSRF 공격 관련 보안 설정
      - Middleware
        - 공통 서비스 및 기능을 애플리케이션에 제공하는 소프트웨어
        - 데이터 관리, 애플리케이션 서비스, 메시징, 인증 및 API 관리를 주로 미들웨어를 통해 처리



- TIP: No Reverse Match 에러는 url만 보면된다.
  1. 문제가 발생된 페이지(html)문서의 url 태그 확인
  2. 문제가 없으면 url을 정의한 urls.py로 이동



### Admin Site

---

- **Automatic admin interface**

  - 사용자가 아닌 서버의 관리자가 활용하기 위한 페이지
  - Model class를 admin.py에 등록하고 관리
  - django.contrib.auth 모듈에서 제공됨
  - record 생성 여부 확인에 매우 유용하며, 직접 record를 삽입할 수도 있음

- **admin 생성**

  ```bash
  $ python manage.py createsuperuser
  ```

  - 관리자 계정 생성 후 서버를 실행한 다음 '/admin'
  - 내가 만든 Model을 보기 위해서는 admin.py에 작성
  - [주의] auth에 관련된 기본 테이블이 생성되지 않으면
  - 데이터베이스에 저장됨

- **admin 등록**

  ```python
  # articles/admin.py
  ```
