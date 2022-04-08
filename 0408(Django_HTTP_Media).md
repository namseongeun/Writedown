# Django: HTTP requests / Media Files

## Handling HTTP requests

- Django에서 **HTTP 요청을 처리**하는 방법

  1. **Django shortcut functions**

     - django.shortcuts 패키지는 개발에 도움이 될 수 있는 여러 함수와 클래스를 제공

     - **shortcut function 종류**

       - **render()**

         - 만약 render()가 없다면...

         ```python
         return HttpResponse(template.render(context, request))
         ```

       

       - **redirect()**

       

       - **get_object_or_404()**

         - 모델 manager인 objects에서 get()을 호출하지만, 해당 객체가 없을 경우 DoesNotExist예외 대신 Http 404를 raise
         - get()에 경우 조건에 맞는 데이터가 없을 경우에 예외를 발생시킴
           - 코드 실행 단계에서 발생한 예외 및 에러에 대해서 브라우저는 http status code 500으로 인식함
         - 상황에 따라 적절한 예외처리를 하고 클라이언트에게 올바른 에러상황을 전달하는 것 또한 개발의 중요한 요소 중 하나!

         ```python
         from django.shortcuts import render, redirect, get_object_or_404
         
         
         def detail(request, pk):
             article = get_object_or_404(Article, pk=pk)
             context = {
                 'article': article,
             }
             return render(request, 'articles/detail.html', context)
         ```

         - [참고] get_object_or_404()가 없다면

         ```python
         from django.http import Http404
         
         
         def detail(request, pk):
             try:
                 article = Article.objects.get(pk=pk)
             except Article.DoesNotExist:
                 raise Http404('No Article matches the given query.')
             context = {
                 'article': article,
             }
             return render(request, 'articles/detail.html', context)
         ```

         

       - **get_list_or_404()**

         - API 형태에서는 적합하나 게시판 형태에서는 부적합하다.

     

  2. **Django View decorators** 

     - Django는 다양한 HTTp 기능을 지원하기 위해 view 함수에 적용할 수 있는 여러 데코레이터를 제공
     - [참고] **Decorator** (데코레이터)
       - 어떤 함수에 기능을 추가하고 싶을 때 해당 함수를 수정하지 않고 기능을 연장해주는 함수
       - 즉, 원본 함수를 수정하지 않으면서 추가 기능만을 구현할 때 사용

     ```python
     def myWrapper(func):
         def myInnerFunc():
             print("inside wrapper")
             func()
         return myInnerFunc
     
     
     @myWrapper
     def myFunc():
         print("Hello, World!")
         
     
     myFunc()    
     ```

     

     - **Allowed HTTP methods**

       - 요청 메서드에 따라 view 함수에 대한 엑세스를 제한

       - 요청이 조건을 충족시키지 못하면 HttpResponseNotAllowed을 return (405)

       - require_http_methods(), require_POST(), require_safe() 그리고 require_GET()

         1. **require_http_methods()**

            - view 함수가 특정한 method 요청에 대해서만 허용하도록 하는 데코레이터

            ```python
            form django.views.decorators.http import require_http_methods
            
            
            @require_http_methods(["GET", "POST"])
            def my_view(request):
                pass
            ```

         2. **require_POST()**

            - view 함수가 POST method 요청만 승인하도록 하는 데코레이터

         3. **require_safe()**  (ex: index, detail 함수)

            - view 함수가 GET 및 HEAD method만 허용하도록 요구하는 데코레이터
            - django는 require_GET 대신 require_sage를 사용하는 것을 권장

     

     - **실습파일에서의 View decorator 작성예시**

     ```python
     from django.views.decorators.http import require_http_methods, require_POST, require_safe
     
     @require_safe
     def index(request):
         pass
     
     @require_http_methods(['GET','POST'])
     def create(request):
         pass
     
     @require_safe
     def detail(request, pk):
         pass
     
     @require_POST
     def delete(request, pk):
         pass
     
     @require_http_methods(['GET', 'POST'])
     def update(request, pk):
         pass
     ```



## Media files

- 미디어 파일
- 사용자가 **웹에서 업로드하는 정적파일(user_uploaded)**
- 유저가 업로드한 모든 정적파일



- **ImageField()**

  - **이미지 업로드에 사용하는 모델 필드**
  - **FileField를 상속받는 서브 클래스**이기 때문에 **FileField의 모든 속성 및 메서드를 사용 가능**하며, 더해서 사용자에 의해 **업로드된 객체가 유효한 이미지인지 검사**함
  - ImageField 인스턴스는 최대 길이가 100자인 문자열(이미지의 파일이 아닌 **경로)로 DB에 생성**되며, max_length 인자를 사용하여 최대 길이를 변경할 수 있음
  - 사용하려면 **반드시 Pillow 라이브러리가 필요**

  

  - **ImageField 작성**

    ```python
    # articles/modes.py
    
    class Article(models.Model):
        title = models.CharField(max_length=10)
        content = models.TextField()
        # saved to 'MEDIA_ROOT/images/'
        image = models.ImageField(upload_to='images/', blank=True)
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    ```

    - **upload_to ='images/'**

      - 실제 이미지가 저장되는 경로를 지정

      - **upload_to의 argument**

        - 업로드 디렉토리와 파일 이름을 설정하는 2가지 방법을 제공

          1. **문자열 값이나 경로 지정**
          2. **함수호출**

        - **문자열 경로 지정 방식**

          - 파이썬의 **strftime() 형식이 포함**될 수 있으며, 이는 파일 업로드 날짜/시간으로 대체 됨

          ```python
          # models.py
          
          class MyModel(models.Model):
              # MEDIA_ROOT/uploads/ 경로로 파일 업로드
              upload = models.FileField(upload_to='uploads/')
              # MEDIA_ROOT/uploads/2022/01/01 경로로 파일 업로드
              upload = models.FileField(upload_to='uploads/%Y/%m/%d/')
          ```

        - **함수 호출 방식**

          - 반드시 2개의 인자(instance, filename)를 사용

          1. **instance**
             - FileField가 정의된 모델의 인스턴스
             - 대부분 이객체는 아직 데이터베이스에 저장되지 않았으므로 PK값이 아직 없을 수 있음
          2. **filename**
             - 기존파일에 제공된 이름

        

    - **blank=True**

      - 이미지 필드에 빈 값(빈 문자열)이 허용되도록 설정(이미지를 선택적으로 업로드 할 수 있도록)
      - 기본값: False
      - True인 경우 필드를 비워 둘 수 있음
        - DB에는 ''(빈 문자열)이 저장됨
      - 유효성 검사에서 사용 됨(is_valid)
        - 모델 필드에 blank=True를 작성하면 form 유효성 검사에서 빈 값을 입력할 수 있음

    

    - **null**
      - 기본값: False
      - True인 경우 django는 빈값에 대해 DB에 NULL로 저장
      - 주의 사항 
        - CharField, TextField와 같은 문자열 기반 필드에는 사용하는 것을 피해야함
        - 문자열 기반 필드에 True로 설정시 '데이터 없음(no data)'에 "빈 문자열(1)"과 "NULL(2)"의 2가지 가능한 값이 있음을 의미하게 된다.
        - 대부분의 경우 "데이터 없음"에 대햇 두 개의 가능한 값을 갖는 것은 중복되는 것이며, Django는 NULL 이 아닌 빈 문자열을 사용하는 것이 규칙

    

    - blank & null 비교

      ```python
      # models.py
      
      class Person(models.Model):
          name = models.CharField(max_length=10)
          # null=True 금지
          bio = models.TextField(max_length=50, blank=True) 
          # null, blank 모두 설정 가능 >> 문자열 기반 필드가 아니기 때문
          birth_date = models.DataField(null=True, blank=True)
      ```

      - blank: 유효성 검사에서 사용
      - null: DB 관련하여 사용



- **FileField()**
  - 파일 업로드에 사용하는 모델 필드
  - 2개의 선택인자를 가지고 있음
    1. upload_to
    2. storage



- **ImageField (or FileField)를 사용하기 위한 몇가지 단계**

  1. settings.py에 MEDIA_ROOT, MEDIA_URL 설정

  2. upload_to 속성을 정의하여 업로드 된 파일에 사용할 MEDIA_ROOT의 하위 경로를 지정

  3. 업로드 된 파일의 경로는 django가 제공하는 'url'속성을 통해 얻을 수 있음

     ```html
     <img src="{{ article.image.url }}" alt="{{ article.image }}">
     ```

  

  - **MEDIA_ROOT**
    - 사용자가 업로드 한 파일(미디어 파일)들을 보관할 디렉토리의 절대 경로
    - django는 성능을 위해 업로드 파일은 데이터베이스에 저장하지 않음
      - 파일의 경로가 저장
    - [주의] MEDIA_ROOT는 STATIC_ROOT와 반드시 다른 경로로 지정해야 함
  - **MEDIA_URL**
    - MEDIA_ROOT에서 제공되는 미디어를 처리하는 URL
    - 업로드 된 파일의 주소를 만들어 주는 역할
      - 웹 서버 사용자가 사용하는 public URL
    - 비어 있지 않은 값으로 설정한다면 반드시 slash(/)로 끝나야 함
    - [주의] MEDIA_URL은 STATIC_URL과 반드시 다른 경로로 지정해야 함

  

  - **개발 단계에서 사용자가 업로드한 파일 제공하기**

    - **settings.MEDIA_URL**
      - 업로드 된 파일의 URL
    - **settings.MEDIA_ROOT**
      - MEDIA_URL을 통해 참조하는 파일의 실제 위치

    ```python
    # crud/urls.py
    
    from django.contrib import admin
    from django.urls import path, include
    from django.conf import settings
    from django.conf.urls.static import static
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('articles/', include('articles.urls')),
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```

    

  - **Pillow 라이브러리 설치 및 마이그레이션 진행**

    ```bash
    $ pip install Pillow
    $ python manage.py makemigrations
    $ python manage.py migrate
    $ pip freeze > requirements.txt
    ```



### Image Upload (CREATE)

---

- **게시글 작성 form 태그에 enctype 속성 지정**

  ```html
  <!-- articles/create.html -->
  
    <form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
  ```

  - form태그 enctype(인코딩) 속성
    1. **multipart/form-data**
       - 파일/이미지 업로드 시에 반드시 사용해야 함(전송되는 데이터의 형식을 지정)
       - `<input type="file">`을 사용할 경우에 사용
    2. application/x-www-form-urlencoded
       - (기본값) 모든 문자 인코딩
    3. text/plain
       - 인코딩을 하지 않은 문자 상태로 전송
       - 공백은 '+'기호로 변환하지만, 특수문자는 인코딩하지 않음 

  

  - input 요소 - accept 속성

    - 입력 허용할 파일 유형을 나타내는 문자열
    - 쉼표로 구분된 '고유 파일 유형 지정자'
    - 파일을 검증하는 것은 아님

    

    - 고유 파일 유형 지정자
      - `<input type="file">`에서 선택할 수 있는 파일의 종류를 설명하는 문자열



- **views.py 수정**

  - 파일을 업로드할 때는 POST가 아니라 **FILES**

  ```python
  def create(request):
      if request.method == 'POST':
          # create
          form = ArticleForm(request.POST, request.FILES)
  
          # 유효성검사: 관리자가 정한 기준을 벗어나면 여기서 걸러짐
          if form.is_valid():
              article = form.save()
              return redirect('articles:detail', article.pk)
  ```



- **출력하기**

  ```html
  <!-- datail.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>DETAIL</h1>
    <h3>{{ article.pk }}번째 글</h3>
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
    <hr>
    <p>제목 : {{ article.title }}</p>
    <p>내용 : {{ article.content }}</p>
    <p>작성 시각 : {{ article.created_at }}</p>
    <p>수정 시각 : {{ article.updated_at }}</p>
    <hr>
    <a href="{% url 'articles:update' article.pk %}">수정</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="삭제">
    </form>
    <a href="{% url 'articles:index' %}">back</a>
  {% endblock content %}
  ```

  

- **STATIC_URL과 MEDIA_URL**
  - static, media 파일 모두 서버에 요청해서 조회하는 것
    - 서버에 요청하기 위해선 url 필요

 

---

**이미지 수정하기**

- 이미지는 바이너리 데이터(하나의 덩어리)이기 때문에 텍스트처럼 일부만 수정하는 것은 불가능
- 때문에 새로운 사진으로 덮어 씌우는 방식을 사용



- **update.html의 form 태그에 enctype 속성 추가**

  ```html
  {% extends 'base.html' %}
  
  
  {% block content %}
    <h1>UPDATE</h1>
    <hr>
    <form action="{% url 'articles:update' article.pk %}" method="POST" enctype="multipart/form-data">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
    <a href="{% url 'articles:detail' article.pk %}">back</a>
  {% endblock content %}
  ```



- **이미지 없이 작성된 게시글이 출력되지 않는 문제 해결하기**

  - image가 없는 게시글의 경우 출력할 이미지 경로가 없기 때문
  - if 태그 사용

  ```html
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>DETAIL</h1>
    <h3>{{ article.pk }}번째 글</h3>
    {% if article.image  %}
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
    {% endif %}
    <hr>
    <p>제목 : {{ article.title }}</p>
    <p>내용 : {{ article.content }}</p>
    <p>작성 시각 : {{ article.created_at }}</p>
    <p>수정 시각 : {{ article.updated_at }}</p>
    <hr>
    <a href="{% url 'articles:update' article.pk %}">수정</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="삭제">
    </form>
    <a href="{% url 'articles:index' %}">back</a>
  {% endblock content %}
  
  ```

  

### Image Resizing

---

**이미지 크기 변경하기**

- 실제 원본 이미지를 서버에 그대로 업로드 하는 것은 서버의 부담이 큰 작업
- `<img>` 태그에서 직접 사이즈를 조정할 수도 있지만 (width와 height 속성 사용), 업로드 될때 이미지 자체를 resizing 하는 것을 고려하기
  - django-imagekit 라이브러리 활용



- **Django-imagekit 설치하기**

  1. **장고 이미지킷 설치**

     ```bash
     $ pip install django-imagekit
     ```

  2. 설치파일을 settings.py / **INSTALLED_APPS에 등록**하기

     ```python
     # crud/settings.py
     
     INSTALLED_APPS = [
         'articles',
         'django_extensions',
         'imagekit',
         'django.contrib.admin',
         'django.contrib.auth',
         'django.contrib.contenttypes',
         'django.contrib.sessions',
         'django.contrib.messages',
         'django.contrib.staticfiles',
     ]
     ```



- **원본 이미지를 재가공하여 저장** (원본이 아니라 썸네일)

  ```python
  # articles/models.py
  
  from email.mime import image
  from imagekit.processors import Thumbnail
  from imagekit.models import ProcessedImageField
  from django.db import models
  
  # Create your models here.
  class Article(models.Model):
      title = models.CharField(max_length=10)
      content = models.TextField()
      # image = models.ImageField(upload_to='images/', blank=True)
      image = ProcessedImageField(
          blank=True,
          upload_to='thumbnails/',
          processors=[Thumbnail(200,300)],
          format='JPEG',
          options={'quality': 90},
      )
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  
      def __str__(self):
          return self.title
  ```

  - 모델 수정 후 마이그래이션은 항상 동기화해주기

