# Django

---

### Web Framework

---

- **Django**
  - 파이썬 웹 프레임워크

- **Web**
  - Word Wide Web
  - 인터넷에 연결된 컴퓨터를 통해 정보를 공유할 수 있는 전세계적인 정보 공간
  - **주요키워드 4가지**
    - **클라이언트**: 네트워크라는 환경을 통해 서버에 정보 요청을 보내는 디바이스 또는 웹브라우저 등
    - **서버**: 클라이언트에게 네트워크 환경을 통해 정보나 시스템 제공
    - **요청**
    - **응답**



- **Static web page (정적 웹 페이지)**
  - 서버에 미리 저장된 파일이 사용자에게 그대로 전달되는 웹페이지
  - 서버가 정적 웹페이지에 대한 요청을 받은 경우 서버는 추가적인 처리과정 없이 클라이어트에게 응답을 보냄
  - 모든 상황에서 모든 사용자에게 동일한 정보를 표시
  - 일반적으로 HTML, CSS, JavaScript로 작성된다.
  - flat page라고도 함

- **Dynamic web page (동적 웹 페이지)**
  - 웹 페이지에 대한 요청을 받은 경우 서버는 추가적인 처리 과정 이후 클라이언트에게 응답을 보냄
  - 동적 페이지는 방문자와 상호작용하기 때문에 페이지 내용은 그때그때 다름
  - 서버 사이트 프로그래밍 언어 (python, jave, C++ 등)이 사용되며 파일을 처리하고 데이터 베이스와의 상호작용이 이루어짐



- **Framework**
  - 프로그래밍에서 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스와 라이브러리 모임
  - 재사용할 수 있는 수많은 코드를 프레임워크로 통합함으로써 개발자가 새로운 어플리케이션을 위한 표준 코드를 다시 작성하지 않아도 같이 사용할 수 있도록 도움
  - Application Framework 라고도 함



- **Web Framework**
  - 웹페이지를 개발하는 과정에서 겪는 어려움을 줄이는 것이 주 목적으로 데이터베이스 연동, 템플릿 형태의 표준, 세션관리, 코드 재사용 등의 기능을 포함
  - 동적인 웹페이지나 웹 애플리케이션, 웹 서비스 개발 보조용으로 만들어지는  Application framework의 일종



- **Django를 사용해야 하는 이유**
  - 검증된 Python 언어 기반 Web framework
  - 대규모 서비스에도 안정적이며 오랫동안 세게적인 기업들에 의해 사용됨



- **Framework Architecture**
  - **MVC Design Parttern** (model - view - controller)
  - 소프트웨어 공학에서 사용되는 디자인 패턴 중 하나
  - 사용자 인터페이스로부터 프로그램 로직을 분리하여 애플리케이션의 시각적 요소나 이면에서 실행되는 부분을 서로 영향없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있음
  - Django는 MTV Pattern이라고 함



- **MTV Pattern**
  - **Model**
    - 응용 프로그램의 데이터 구조를 정의하고 데이터 베이스의 기록을 관리 (추가, 수정, 삭제)
  - **Template (view)**
    - 파일의 구조나 레이아웃을 정의
    - 실제 내용을 보여주는데 사용 (presentation)
  - **View (controller)**
    - HTTP 요청을 수신하고 HTTP 응답을 반환
    - Model을 통해 요청을 충족시키는 데 필요한 데이터에 접근
    - template에게 응답의 서식 설정을 맡김



- **MTV Pattern** (순서 이해가 굉장히 중요하다!)
  1. **HTTP Request**
  2. **URLS**
  3. **Veiw**
  4. **HTTP Response**



### Django Intro

---

- **주의사항**
  - 프로젝트 이름에는 Python이나 Django에서 사용중인 키워드를 피해야한다.
  - 하이픈(`-`)은 사용할 수 없다.



- **Django 시작하기**

  1. **가상환경 생성 및 활성화**

     - 가상환경 생성

     ```bash
     $ python -m venv venv
     ```

     - vscode의 interpreter에서 venv 선택하고 터미널을 켜서 activate 상태 확인

  2. **django 설치**

     - 작업환경 확인 후 장고 설치, 작업환경 저장

     ```bash
     $ pip list
     $ pip install django==3.2.12
     $ pip list
     $ pip freeze > requirements.txt
     ```

  3. **프로젝트 생성**

     ```bash
     $ django-admin startproject 프로젝트이름 .
     ```

  4. **서버켜서 로켓 확인하기** (서버 잘 끄기)

     ```bash
     $ python manage.py runserver
     ```

     - 끄기 : `Ctrl + C`

  5. **앱 생성** (실제 기능들에 대한것을 가지고 있는 것)

     ```bash
     $ python manage.py startapp 앱이름(복수형)
     ```

  6. **앱 등록**

     - project의 settings.py에서 INSTALLED_APPS 에 앱을 등록한다.



- **프로젝트 구조**
  - `__intit__.py`: Python 에게 이 디렉토리를 하나의 Python 패키지로 다루도록 지시
  - `asji.py`(Asynchronous Server Gateway Interface) : Django 애플리케이션이 비동기식 웹서버와 연결 및 소통하는 것을 도움
  - `settings.py`: 애플리케이션의 모든 설정을 포함
  - `urls.py`: 사이트의 url과 적절한 views의 연결을 지정
  - `wsgi.py` (web Serever Gateway Interface) : Django 애플리케이션이 웹서버와 연결 및 소통하는 것을 도움
  - `manage.py` : Django 프로젝트와 다양한 방법으로 상호작용하는 커맨드라인 유틸리티 



- **Application 생성**

  - 일반적으로 Application 명은 복수형으로 하는 것을 권장

    ```$ python manage.py stratapp articles```

- **Application 구조**

  - `admin.py` : 관리자용 페이지를 설정하는 곳
  - `apps.py` : 앱의 정보가 작성된 곳 (건드리지 않음)
  - `models.py`: 애벵서 사용하는 Model을 정의하는 곳
  - `tests.py `: 프로젝트의 테스트 코드를 작성하는 곳 (건드리지 않음)
  - `views.py` : view 함수들의 정의 되는 곳



- **Project & Application**
  - **Project**
    - 프로젝트는 **앱의 집합**
    - 프로젝트에는 여러 앱이 포함될 수 있음
    - 앱은 여러 프로젝트에 있을 수 있음
  - **Application**
    - 앱은 실제 요청을 처리하고 페이지를 보여주고 하는 등의 역할을 담당
    - 하나의 프로젝트는 여러앱을 가짐
    - 일반적으로 앱은 **하나의 역할 및 기능 단위**로 작성한다.



- **Application(앱) 등록**

  - 프로젝트에서 앱을 사용하기 위해서는 반드시 INSTALLED_APPS 리스트에 추가!

  - INSTALLED_APPS: Django installation에 활성화 된 모든 앱을 지정하는 문자열 목록

  - 앱 생성 시 주의 사항

    - 반드시 생성 후 등록!

    - INSTALLED_APPS에 먼저 작성하고 생성하려면 앱이 생성되지 않음

    - Django가 권장하는 앱 등록 순서

      1. Local apps
      2. Third party apps
      3. Django apps

      - 해당 순서를 지키지 않아도 문제는 없지만 추후에는 문제가 생길 수 있기 때문에 지키는 것을 권장한다.



### 요청과 응답 (Request & Response)

---

- **URLs**
  - HTTP 요청(request)을 알맞은 view로 전달
  - path() 함수의 첫번째 주소가 포함되면 두번째 인자로 있는 주소가 서버에서 호출된다.
  - `__init__.py`가 있으므로 템플릿을 불러오는 역할을 해주는 views를 임포트한다.
  - **trailing comma**: Django에서 권장하는 문법으로 마지막에 ,를 찍는다.
  - **view함수의 필수 인자: request**
    - HTTP 요청 객체! request를 필수인자로 받지 않으면 오류 발생
  - **render함수의 첫번째 필수 인자: request**
    - 이것도 마찬가지로 필수!
    - 두번째 인자는 요청을 받고 렌더링할 요소
  - app 안에 templates 안에 html 생성 (약속임)
  - html 탬플릿을 작성한 후에 runserver해서 주소에 붙이면 된다.



**urls 쓰고 views 쓰고 templates 쓰는게 실제 요청을 주고 받는 과정과 동일하기 때문에 이것과 같은 순서로 작성하는 것이 필요하다.**



- **추가설정**
  - **LANGUAGE_CODE**
    - 모든 사용자에게 제공되는 번역을 결정
    - 이 설정이 적용되려면 USE_I18N이 활성화 되어있어야 함
  - **TIME_ZONE**
    - 데이터베이스 연결의 시간대를 나타내는 문자열 지정
    - USE_TZ가 True 이고 이 옵션이 설정된 경우 데이터 베이스에서 날짜 시간을 읽으면 UTC대신 새로 설정한 시간대의 인식 날짜 &시간이 반환됨
    - USE_TZ이 False인 상태로 이 값을 설정하는 것은 에러가 발생하므로 주의
  - **USE_I18N**
    - Django의 번역 시스템을 활성화해야 하는지 여부를 지정
  - **USE_L10N**
    - 데이터의 지역화 된 형식을 기본적으로 활성화할지 여부를 지정
    - True일 경우, Django는 현재 local의 형식을 사용하여 숫자와 날짜를 표시
  - **USE_TZ**
    - datetimes가 기본적으로 시간대를 인식하는지 여부를 지정
    - True일 경우 Django는 내부적으로 시간대 인식 날짜/ 시간을 사용



### Template

---

- **Template**
  - 실제내용을 보여주는데 사용되는 파일
  - 파일의 구조나 레이아웃을 정의 (ex: HTML)
  - Template 파일 의 기본 값은 app 폴더 안의 templates폴더로 지정되어 있음



- **Django Template**
  - 데이터 표현을 제어하는 도구이자 표현에 관련된 로직
  - 사용하는 built-in system
    - Django template language 



- **Django template language (DTL)**
  - Django template 에서 사용하는 built-in template system
  - 조건, 반복, 변수 치환, 필터 등의 기능을 제공
  - 단순히 Python이 HTML에 포함 된 것이 아니며 프로그래밍적 로직이 아니라  프레젠테이션을 표현하기 위한 것
  - Python처럼 일부 프로그래밍 구조(if, for 등)를 사용할 수 있지만, 이것은 해당 Python 코드로 실행되는 것이 아님



- **DTL Syntax - Variable**

  ```django
  {{ variable }}
  ```

  - render()를 사용하여 views.py에서 정의한 변수를 template 파일로 넘겨 사용하는 것
  - 변수명은 영어, 숫자와 밑줄(_)의 조합을 구성될 수 있으나 밑줄로는 시작할 수 없음
    - 공백이나 구두점 문자 또한 사용할 수 없음
  - dot(.)를 사용하여 변수 속성에 접근할 수 있음
  - render()의 세번째 인자로 {'key': value}와 같 이 딕셔너리 형태로 넘겨주며, 여기서 정의한 key에 해당하는 문자열이 template에서 사용가능한 변수명이 됨
  - 키와 값의 이름을 같게 해주는 것이 편하다.



- **DTL Syntax - Filters**

  ```django
  {{ variable|filter }}
  ```

  - 표시할 변수를 수정할 때 사용

  - 예를 들어 name 변수를 모두 소문자로 출력하고 싶은 경우, `{{ name|lower}}` 형태로 사용

  - 60개의 built-in template filters를 제공

  - chained가 가능하며 일부 필터는 인자를 받기도 한다. 

    `{{ variable|truncateword:30 }}`



- **DTL Syntax - Tags**

  ```django
  {% tag %}
  ```

  - 출력 텍스트를 만들거나 반복 또는 논리를 수행하여 제어 흐름을 만드는 등 변수보다 복잡한 일들을 수행

  - 일부 태그는 시작과 종료태그가 필요

    `{% if %}{% endif %}`

  - 약 24개의 built-in template tags를 제공



- **DTL Syntax - Comments**

  ```django
  {# 한줄 주석 #}
  
  {% comment %}
  여러줄 주석
  {% endcomment %}
  ```

  

- **Template inheritance** (템플릿 상속)

  - 템플릿 상속은 기본적으로 코드의 **재사용성에 초점**을 맞춤

  - 템플릿 상속을 사용하면 사이트의 모든 공통 요소를 포함하고, 하위 템플릿이 재정의(override)할 수 있는 블록을 정의하는 기본 'skeleton'템플릿을 만들 수 있다.

  - **Template inheritance - "tags"**

    ```django
    {% extends '' %}
    ```

    - 자식(하위) 템플릿이 부모 템플릿을 확장한다는 것을 알림
    - 반드시 템플릿 최상단에 작성되어야 함

    ```django
    {% block content %} {% endblock content %}
    ```

    - 하위 템플릿에서 재지정(overridden)할 수 있는 블록을 정의
    - 즉, 하위 템플릿이 채울 수 있는 공간

  - **templates를 articles와 프로젝트와 동급에 생성하고 settings.py에서 템플릿, DIRS 에 설정**

    ```django
    'DIRS': [BASE_DIR / 'templates'],
    ```

    - 구동되는 운영체제에 맞춰서 알아서 파일 경로가 번역이 되는 특징이 있다.

  - **Template Tag - "include"**

    ```django
    {% include '' %}
    ```

    - 템플릿을 로드하고 현재 페이지로 렌더링
    - 템플릿 내에 다른 템플릿을 포함하는 방법



- **Templates 정리**
  - **DTL (장고의 문법)**: 파이썬과 유사하게 이름을 맞춘 경우가 많지만 파이썬을 사용하는 것이 아니다.
    - **variable**
    - **filter**
    - **tag**
    - **comment**
  - **약속된 경로** - `'app/templates'`
  - **추가 템플릿 경로** - `settings.py` > `TEMPLATES` > `DIRS`
  - **상속**
    - DRY (Dont Repeat Yourself)
    - extends (반드시 문서의 최상단에 위치해야한다.) & block (상속받은 문서 내에 코드를 적을수있는 구역)
  - include tag



- `view.py`에서 인자를 request 가 아닌 다른 것으로 해도 작동한다.
- 파이썬의 문법을 지키기 때문이다. 다만 장고 위에서 지키기로한 일종의 약속이기에 request로 쓴다.



### HTML Form

---

- **HTML ' form' element**
  - 웹에서 사용자 정보를 입력하는 여러방식을 제공하고 사용자로부터 할당된 데이터를 서버로 전송하는 역할을 담당
- **HTML 'input' element**
  - 사용자로부터 데이터를 입력받기 위해 사용 
  - type 속성에 따라 동작 방식이 달라짐
  - **핵심 속성(attribute)**
    - name
    - 중복가능, 양식을 제출했을 때 name이라는 이름에 설정된 값을 넘겨서 값을 가져올 수 있음
    - 주요 용도는 GET/POST 방식으로 서버에 전달하는 파라미터(name은 key, value는 value)로 매핑하는 것
    - GET 방식에서는 URL에서 `?key=value&key=value` 형식으로 데이터를 전달함 (쿼리스트링 파라미터)
- **HTML 'label' element**
  - 사용자 인터페이스 항목에 대한 설명을 나타냄
  - 주요이점
    - 사용자가 입력해야하는 텍스트가 무엇인지 더쉽게 이해할 수 있도록 돕는 프로그래밍적 이점
    - 라벨을 클릭해서 input에 초점을 맞춤
- **HTML 'for' attribute**
  - for 속성의 값과 일치하는 id 를 가진 문서의 첫 번째 요소를 제어
- **HTML 'id' attribute**
  - 전체 문서에서 고유해야하는 식별자를 정의
  - 사용 목적
    - linking, scripting, styling 시 요소를 식별



- **HTTP**
  - **HyperText Transfer Protocol**
  - **웹에서 이루어지는 모든 데이터 교환의 기초**
  - 주어진 리소스(data)가 수행할 작업을 나타내는 **request methods(조회, 생성, 수정, 삭제)를 정의**
  - HTTP request method 종류
    - **GET(조회), POST(생성), PUT, DELETE(삭제) ...등**



- **HTTP request method - "GET"**
  - 서버로부터 정보를 조회하는 데 사용
  - 데이터를 가져올 때만 사용해야 함
  - 데이터를 서버로 전송할 때 body가 아닌 Query Sting Parameters를 통해 전송
  - 우리는 서버에 요청을 하면 HTML 문서 파일 한 장을 받는데 이 때 사용하는 요청의 방식이 GET



### URL

---

- **Django URLs**
  - **dispatcher(운송자)로서의 URL**
  - 웹 애플리케이션은 URL을 통한 **클라이언트의 요청으로부터 시작**



- **Variable Routing**

  - **URL 주소를 변수로 사용**하는 것

  - URL의 일부를 변수로 지정하여 view 함수의 인자로 넘길 수 있음

  - 즉, 변수 값에 따라 하나의 path()에 여러 페이지를 연결시킬 수 있음

    ```python
    /lotto/<int:number>/
    ```

    

- **URL Path converters**

  - **str**
    - '/를 제외하고 비어있지 않은 모든 문자열과 매치
    - 작성하지 않을 경우 기본 값
  - **int**
    - 0 또는 야의 정수와 매치
  - **slug**
    - ASCII 문자 또는 숫자, 하이픈 및 밑줄 문자로 구성된 모든 슬러그 문자열과 매치
  - **uuid**
  - **path**



- **APP URL mapping**

  - **app의 view 함수가 많아지면서 사용하는 path() 또한 많아지고, app 또한 더 많이 작성되기 때문에 프로젝트의 urls.py에서 모두 관리하는 것은 프로젝트 유지보수에 좋지 않다.**

  - 이제는 **각 app에 urls.py를 작성**하게 된다.

    ```python
    # django는 명시적 import 상대경로를 권장
    from . import views
    ```

  - pjt에서 각 앱의 urls로 요청을 연결시켜주는 경로를 만드는 과정이 필요하다!

    - path와 같은 위치에 있는 include 함수 import

    ```python
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('articles/', include('[앱이름].urls')),
    ]
    ```

  - **include()**

    - 다른 URLconf들을 참조할 수 있도록 도움

  - url 태그로 하드코딩이 아닌 이름 경로로 지정가능



### 과정 (throw - catch)

---

1. /throw/ url로 요청을 보냄
2. django의 /throw/ urls.py가 throw view함수를 호출 -> throw 템플릿을 렌더링
3. throw 템플릿을 응답받아서 화면을 볼 수 있음
4. throw 템플릿의 form 태그를 통해 데이터를 /catch/로 submit(제출)
5. django의 /catch/ urls.py가 catch view 함수를 호출
6. catch view 함수는 요청 객체의 데이터를 추출
7. catch view함수는 추출한 데이터와 함께 catch 템플릿을 응답(렌더링)
8. 클라이언트는 응답 받은 catch 템플릿을 보게 됨



### Namespace

---

- 두가지 앱의 페이지 이름을 똑같이 하면 생기는 2가지 문제
  1.  aticles 앱의 indes페이지에서 두번째 앱 pages의 index로 이동하는 하이퍼링크를 클릭시 현재 페이지로 이동됨
      - URL namespace
  2.  pages앱 index url로 이동해도 articles앱의 index 체이지가 출력됨
      - Templates namespace



- **namespace**(이름공간)
  - 이름공간은 객체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 객체만을 가리키게 된다.
  - 프로그래밍을 하다보면 모든 변수명과 함수명 등 이들 모두를 겹치지 않게 정의하는 것은 매우 어려운 일
  - 그래서 django에서는
    1. 서로 다른 app의 같은 이름을 가진 url name은 이름공간을 설정해서 구분
    2. templates, static 등 django는 정해진 경로 하나로 모아서 보기 때문에 중간에 폴더를 임의로 만들어 줌으로써 이름 공간을 설정



- templates폴더 내에 새로운 폴더를 만들고 그 안에 템플릿들을 넣어서 이름공간을 물리적으로 만들어준다.

  대신 모든 경로를 전부 바꿔줘야 한다. 정말 저어어언부



- **URL namespace**

  - URL namespace를 사용하면 서로 다른 앱에서 동일한 URL 이름을 사용하는 경우에도 이름이 지정된 URL을 고유하게 사용할 수 있음

  - urls.py에 'app_name' attribute 값 작성

  - 참조는 `:`연산자를 사용하여 지정한다.

    ```
    <urls.py>
    app_name = "articles"
    
    <html>
    <a href="{% url 'articles:index '%}">메인페이지</a> 
    ```

- **Template namespace**

  - pjt의 settings.py에 등록된 앱 순서로 우선순위를 따진다.
  - 임의로 templates의 폴더 구조를 `app_naem/templates/app_name`형태로 변경해 임의로 이름 공간을 생성 후 변경된 추가경로로 수정



### Static Files

---

- **웹서버와 정적파일**
  - 웹서버는 특정 위치에 있는 자원을 요청(HTTP request)받아서 제공하는 응답(HTTP response)을 처리하는 것을 기본 동작으로 한다.

  - 이는 자원과 접근 가능한 주소가 정적으로 연결된 관계
    - 예를 들어 사진 파일은 자원이고 파일 경로는 웹 주소라 함

  - 즉, 웹 서버는 요청받은 URL로 서버에 존재하는 정적자원(static resource)를 제공



- **Static file**
  - 정적 파일
  - 응답할 때 별도의 처리 없이 파일 내용을 그대로 보여주면 되는 파일
    - 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 보여주는 파일
  - 예를 들어 웹서버는 일반적으로 이미지, 자바 스크립트 또는 CSS와 같은 미리 준비된 추가파일(움직이지 않는)을 제공해야함
  - 파일 자체가 고정되어 있고, 서비스 중에도 추가되거나 변경되지 않고 고정되어 있음
  - Django에서는 이러한 파일들을 'Static file'이라 함
    - Django는 staticfiles 앱을 통해 정적파일과 관련된 기능을 제공



- **Static files 구성**

  1. django.contrib.staticfiles가 INSTALLED_APPS에 포함되어 있는지 확인

  2. settings.py에서 STATIC_URL을 정의

  3. 템플릿에서 static 템플릿 태그를 사용하여 지정된 상대경로에 대한  URL을 빌드 

     ```html
     {% load static %}
     
     <img src="{% static 'my_app/example.jpg' %}" alt="My image">
     ```

  4. 앱의 static 디렉토리에 정적 파일을 저장

  - static도 templates와 같은 경로방식으로 동작하기 때문에 별도의 이름공간을 만들어주는 것이 필요하다.



- **The staticfiles app**

  - **STATICFILES_DIRS**

    - 'app/static/' 디렉토리 경로(기본 경로)를 사용하는 것 외에 추가적인 정적 파일 경로 목록을 정의하는 리스트

    - 추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목록으로 작성되어야 함

      ```python
      STATICFILES_DIRS = [BASE_DIR / 'static']
      ```

  - **STATIC_URL**

    - STATIC_ROOT에 있는 정적 파일을 참조 할 때 사용할 URL

      - 개발 단계에서는 실제 정적 파일들이 저장되어 있는 'app/static'경로 및 STATICFILES_DIRS에 정의된 추가 경로들을 탐색함
      - 실제 파일이나 디렉토리가 아니며, URL로만 존재 (요청과 응답을 위해 URL로 존재)
      - 비어있지 않은 값으로 설정한다면 반드시 slash(/)로 끝나야 함

      ```python
      STATIC_URL = '/static/'
      ```

  - **STATIC_ROOT (배포단계에서만 동작한다.)**

    - collectstatic이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로

    - django프로젝트에서 사용하는 모든 정적 파일을 한데 모아 놓는 경로

    - 개발 과정에서 settings.py의 DEBUG 값이 True로 설정되어 있으면 해당 값은 작용되지 않음

      - 직접 작성하지 않으면 django 프로젝트에서는 settigns.py에 작성되어 있지 않음

    - 실 서비스 환경(배포환경)에서 django의 모든 정적 파일을 다른 웹 서버가 직접 제공하기 위함

      ```python
      STATIC_ROOT = BASE_DIR / 'statcifiles'
      ```

      ```bash
      $ python manage.py collectstatic  
      ```

      

- **Django template tag**

  - **load**

    - 사용자 정의 템플릿 태그 세트를 로드(load)
    - 로드하는 라이브러리, 패키지에 등록된 모든 태그와 필터를 불러옴

  - **static**

    - STATIC_ROOT에 저장된 정적 파일에 연결

    ``` _ㅔㅛhtml
    {% load static %}
    
    <img src="{% static 'my_app/example.jpg' %}" alt="My image">
    ```
