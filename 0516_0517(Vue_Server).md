# Vue - Server & Client

## Server & Client

##### Server

- **클라이언트에게 '정보', '서비스'를 제공**하는 컴퓨터 시스템
- 정보 & 서비스
  - Django를 통해 응답한 **template**
  - DRF를 통해 응답한 **JSON**



##### Client

- 서버에게 그 서버가 맡는 (서버가 제공하는) **서비스를 요청**하고, 
- 서비스 요청을 위해 필요한 인자를 **서버가 요구하는 방식에 맞게 제공**하며, 
- 서버로부터 반환되는 응답을 **사용자에게 적절한 방식으로 표현**하는 기능을 가진 시스템 



##### 정리

- Server는 '정보제공'
  - DB와 통신하며 데이터를 CRUD
  - 요청을 보낸 Client에게 이러한 정보를 응답
- Client는 '정보 요청 & 표현'
  - Server에게 정보(데이터) 요청
  - 응답 받는 정보를 잘 가공하여 화면에 보여줌



## CORS

### SOP (Same-origin policy)

- 동일 출처 정책
- 특정 출처(origin)에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 보안 방식
- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격 받을 수 있는 경로를 줄임



##### Origin (출처)

- 두 URL의 Protocol, Port, Host가 모두 같아야 동일한 출처라 할 수 있음

  ![image-20220516142456494](0516_0517(Vue_Server).assets/image-20220516142456494.png)



### CORS (Cross-Origin Resource Sharing)

- 교차 출처 리소스(자원) 공유
- 추가 HTTP header를 사용하여 특정 출처에서 실행중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
- 리소스가 자신의 출처(Domain, Protocol, Port)와 다를 때 교차 출처 HTTP 요청을 실행
- 보안 상의 이유로 브라우저는 교차 출처 HTTP  요청을 제한(SOP)
  - 예를 들어 XMLHttpTequest는 SOP를 따름
- 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS header를 포함한 응답을 반환해야 함



##### 교차 출처 접근 허용하기

- CORS를 사용해 교차 출처 접근을 허용하기 
- CORS 는 HTTP의일부로 어떤 호스트에서 자신의 컨텐츠를 불러갈 수 있는지 서버에 지정할 수 있는 방법



##### Why CORS ?

1. 브라우저 & 웹 애플리케이션 보호
   - 악의적인 사이트의 데이터를 가져오지 않도록 사전 차단
   - 응답으로 받는 자원에 대한 최소한의 검증
   - 서버는 정상적으로 응답하지만 브라우저에서 차단
2. Server의 자원 관리
   - 누가 해당 리소스에 접근할 수 있는지 관리 가능



##### How CORS ?

- CORS 표준에 의해 추가된 HTTP Header를 통해 이를 통제

- CORS HTTP 응답 헤더 예시

  ```http
  Access-Control-Allow-Origin /
  Access_Control-Allow-Credentials /
  Access_Control-Allow-Headers / 
  Access_Control-Allow-Methods
  ```



##### Access-Control-Allow-Origin 응답헤더

- 이 응답이 주어진 출처(origin)로 부터 요청 코드와 공유될 수 있는지 나타냄
- 예시
  - `Access-Control-Allow-Origin: *`
  - 브라우저 리소스에 접근하는 임의의 origin으로부터 요청을 허용한다고 알리는 응답에 포함
  - `'*'`는 모든 도메인 접근할 수 있음을 의미
  - `'*'`외에 특정 origin 하나를 명시 할 수 있음



##### CORS 시나리오 예시

- 요청 헤더의 Origin을 보면 localhost:8080 을로부터 요청이 왔다는 것을 알 수 있음

- 서버는 이에 대한 응답으로 Access-Control-Allow-Origin 헤더를 다시 전송

- 만약 서버 리소스 소유자가 오직 `localhos:8080`의 요청에 대해서만 리소스에 대한 접근을 허용하려는 경우, `'*'`가 아닌 `Access-Control-Allow-Origin: localhost:8080`을 전송해야 함

  ![image-20220516143930610](0516_0517(Vue_Server).assets/image-20220516143930610.png)

  1.  Vue.js에서 A 서버로 요청
  2.  A 서버는 `Access-Control-Allow-Origin`에 특정한 origin을 포함시켜 응답
      - 서버는 CORS Policy와 직접적인 연관이 없고 그저 요청이 응답함
  3.  브라우저는 응답에서 `Access-Control-Allow-Origin`을 확인 후 허용 여부를 결정
  4.  프레임워크 별로 이를 지원하는 라이브러리가 존재
      - django는 `djngo-cors-headers`라이브러리를 통해 응답 헤더 및 추가 설정 가능



##### Django-cors-headers 라이브러리

- 응답에 CORS header를 추가해주는 라이브러리
- 다른 출처에서 보내는 Django 애플리케이션에 대한 브라우저 요청을 허용함
- Django App이 header 정보에 CORS를 설정한 상태로 응답을 줄 수 있게 도와주며, 이 설정을 통해 브라우저는 다른 origin에서 요청을 보내는 것이 가능해짐



1. ```bash
   $ pip install django-cors-headers
   ```

2. `INSTALLED_APPS`에 추가

   ```python
   INSTALLED_APPS = [
       ...
       'corsheaders'
       ...
   ]
   ```

3. `MIDDLEWARE` 추가

   ```python
   MIDDLEWARE = [
       ...
       # CommonMiddleware보다 위에 위치
       'corsheaders.middleware.CorsMiddleware',
       ...
       'django.middleware.common.CommonMiddelware'
       ...
   ]
   ```

4. `CORS_ALLOWED_ORIGINS`에 교차 출처  자원 공유를 허용할 Domain 등록

   ```python
   CORS_ALLOWED_ORIGINS = [
       'http://localhost:8080',
   ]
   ```



## Authentication & Authorization

##### Authentication

- 인증, 입증
- 자신이라고 주장하는 사용자가 누구인지 확인하는 행위
- 모든 보안 프로세스의 첫 번째 단계 (가장 기본 요소)
- 즉, 내가 누구인지를 확인하는 과정
- 401 Unauthorized
  - 비록 HTTP 표준에서는 미승인을 하고 있지만 의미상 이 응답은 비인증을 의미



##### Authorization

- 권한 부여, 허가
- 사용자에게 특정 리소스 또는 기능에 대한 액세스 권한을 부여하는 과정(절차)
- 보안 환경에서 권한 부여는 항상 인증을 따라야 함
- 서류의 등급, 웹페이지에서 글을 조회 & 삭제 & 수정 할 수 있는 방법, 제한 구역
  - 인증이 되었어도 모든 권한을 부여 받는 것은 아님
- 403 Forbidden
  - 401과 다른 점은 서버는 클라이언트가 누구인지 알고 있음



##### Authentication vs Authorization

- **Authentication(인증)**
  - 자신이라고 주장하는 유저 확인
  - Credentials(비밀번호, 얼굴인식) 검증
  - Django -> 게시판 서비스 로그인
  - 인증 이후에 획득하는 권한(생성, 수정, 삭제)

- **Authorization(권한/허가)**
  - 유저가 자원에 접근할 수 있는지 여부 확인
  - 규칙/규정에 의해 접근할 수 있는지 확인
  - DJango -> 일반 유저 vs 관리자 유저
  - 인증 이후에 부여되는 권한
    - 예시 - 로그인 후 글 작성 여부



##### Authentication and authorization work together

- 회원 가입을 하고 로그인을 하면 할 수 있는 권한 생성
  - 인증 이후에 권한이 따라오는 경우가 많음
- 단, 모든 인증을 거쳐도 권한이 동일하게 부여되는 것은 아님
  - Django에서 로그인을 했더라도 다른 사람의 글까지 수정/삭제가 가능하진 않음
- 세션, 토큰, 제 3자를 활용하는 등의 다양한 인증 방식이 존재



### DRF Authentication

---

#### 다양한 인증 방식

1. Session Based
2. Token Based
   - Basic Token
   - JWT
3. Oauth
   - google
   - facebook
   - github
   - ...



#### Basic Token Authentication

![image-20220516151544144](0516_0517(Vue_Server).assets/image-20220516151544144.png)





#### JWT

![image-20220516151707244](0516_0517(Vue_Server).assets/image-20220516151707244.png)

**JSON Web Token**

- JSON 포맷을 활용하여 요소 간 안전하게 정보를 교환하기 위한 표준 포맷
- 암호화 알고리즘에 의한 디지털 서명이 되어있기 때문에 JWT 자체로 검증 가능
- JWT 자체가 필요한 정보를 모두 갖기 때문에 이를 검증하기 위한 다른 검증 수단이 필요없음
- 사용처
  - Authentication, Information Exchange



**JWT 특징**

- 기본 토큰 인증 체계와 달리 JWT 인증 확인은 데이터베이스를 사용하여 토큰의 유효성을 검사할 필요가 없음
  - 즉, JWT는 데이터베이스에서 유효성 검사가 필요없음
  - JWT 자체가 인증에 필요한 정보를 모두 갖기 때문(self-contained)
  - 이는 세션 혹은 기본 토큰을 기반으로 한 인증과의 핵심 차이점
  - 토큰 탈취시 서버 측에서 토큰 무효화가 불가능 (블랙리스팅 테이블 활용)
  - 매우 짧은 유효기간(5min)과 Refresh 토큰을 활용하여 구현
  - MSA(Micro Server Architecture) 구조에서 서버간 인증에 활용
  - One Source(JWT) Multi Use 가능



`dj-rest-auth` **&** `django-allauth`라이브러리

- 설치

  ```bash
  $ pip install django-allauth
  $ pip install dj-rest-auth
  ```

- settings.py

  ```python
  INSTALLED_APPS = [
      ...
      'rest_framework',
      # token authentication
      'rest_framework.authtoken',
      
      # DRF auth 담당
      'dj_rest_auth',
      'dj_rest_auth.registration',
      
      # django allauth
      'allauth',
      'allauth.account',
      ...
  	# allauth 사용을 위해 필요
      'django.contrib.sites',
      ...
  ]
  
  # django.contrib.sites 에서 등록 필요
  SITE_ID = 1
  
  # drf 설정
  REST_FRAMEWORK = {
      # 기본 인증 방식 설정
      'DEFAULT_AUTHENTICATION_CLASSES': [
          'rest_framework.authentication.TokenAuthentication',
      ],
      
      # 기본 권한 설정
      'DEFAULT_PERMISSION_CLASSES': [
          # 'rest_framework.permissions.AllowAny',		# 모두에게 허용
          'rest_framework.permissions.IsAuthenticated',	# 인증된 사용자에게 허용
      ],
  }
  ```

- urls.py

  ```python
  urlpatterns = [
      path('admin/', admin.site.urls),
      ...
      # 패턴은 자유롭게 설정 가능. 포워딩만 dj_rest_auth로
      path('api/v1/accounts/', include('dj_rest_auth.urls')),
      path('api/v1/accounts/signup/', include('dj_rest_auth.registration.urls')),
  ]
  ```

  
