# DataBase - Model Relationhsip

## Foreign Key

**Foreign Key 개념**

- 외래 키 (외부 키)
- 관계형 데이터베이스에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
- 참조하는 테이블에서 속성(필드)에 해당하고 이는 참조되는 테이블의 기본키를 가리킴
- 참조하는 테이블의 외래 키는 참조되는 테이블 행 1개에 대응됨
  - 이 때문에 참조하는 테이블에서 참조되는 테이블의 존재하지 않는 행을 참조할 수 없음
- 참조하는 테이블의 행 여러 개가 참조되는 테이블의 동일한 행을 참조할 수 있음
- 예시: 게시글과 댓글 간의 모델 관계 설정
  - 참조하는 모델(Comment)에서 외래 키는 참조되는 모델(Article)의 기본키를 가리킴



**Foreign Key  특징**

- 키를 사용하여 부모 테이블의 **유일한 값을 참조** (참조 무결성)
- 외래 키의 값이 반드시 부모테이블의 기본 키일 필요는 없지만 유일한 값이어야 함
- [참고] 참조 무결성
  - 데이터베이스 관계 모델에서 **관련된 2개의 테이블 간의 일관성**을 말함
  - **외래 키가 선언된 테이블의 외래 키 속성(열)의 값은 그 테이블의 부모가 되는 테이블의 기본 키 값으로 존재해야 함**



**`ForeignKey` field**

- A **mane-to-one relationship**
- 2개의 위치 인자가 반드시 필요
  1. 참조하는 model class
  2. on_delete 옵션
- **migrate 작업 시 필드 이름에 _id를 추가하여 데이터베이스 열 이름을 만듦**



- **comment 모델 정의**하기 (Migrate 포함)

  ```python
  # articles / models.py
  
  class Comment(models.Model):
      article = models.ForeignKey(Article, on_delete=models.CASCADE)
      content = models.CharField(max_length=200)
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
      
      def __str__(self):
          return self.content
  ```

  - 주의! - 틀리는 상황

    ```python
    class Comment(models.Model):
        article_id = models.ForeignKey(Article, on_delete=models.CASCADE)
        # Comment 모델은 외래키 article_id를 포함한다.
        # 이말 때문에 이렇게 쓰면 안된다!
        # 이렇게 쓰면 생성 결과가 '_id_id' 이렇게 된다.
        # 그리고 소문자 단수형으로 쓰는 이유는 1:N 관계임을 명시하고 어떤 것을 참조하는지 정확히 알리기 위함이다.
    ```



- **`ForeignKey`**arguments - '**on_delete**'
  - **외래 키가 참조하는 객체가 사라졌을 때 외래키를 가진 객체를 어떻게 처리할 지를 정의**
  - **Database Integrity(데이터 무결성)**을 위해서 매우 중요한 설정
  - on_delete 옵션에 사용가능한 값들
    - **CASCADE: 부모 객체(참조 된 객체)가 삭제 됐을 때 이를 참조하는 객체도 삭제**
    - PROTECT
    - SET_NULL
    - SET_DEFAULT
    - SET()
    - DO_NOTHING
    - RESTRICT
  - **[참고] 데이터 무결성**
    - 데이터의 **정확성과 일관성을 유지하고 보증**하는 것을 가리키며 데이터베이스나 RDBMS 시스템의 중요한 기능
    - 무결성 제한의 **유형**
      1. **개체 무결성 (Entity integrity)**
         - PK의 개념과 관련
         - 모든 테이블이 PK를 가져야 하며 PK로 선택된 열은 고유한 값이어야 하고 빈 값은 허용치 않음을 규정
      2. **참조 무결성 (Referential integrity)**
         - FK 개념과 관련
         - FK 값이 데이터베이스의 특정 테이블의 PK 값을 참조하는 것
      3. **범위 무결성 (Domain integrity)**
         - 정의된 형식에서 관계형 데이터베이스의 모든 컬럼이 선언되도록 규정



- **데이터베이스의 ForignKey 표현**
  - 만약 ForignKey 인스턴스를 abcd로 생성했다면 컬럼명은 abcd_id로 만들어짐
  - 하지만 명시적인 모델관계 파악을 위해 참조하는 클래스 이름의 소문자(단수형)로 작성하는 것이 바람직하다.



- **댓글 생성 연습하기**

  - 쉘플러스 실행

    ```bash
    $ python manage.py shell_plus
    ```

  - 게시글 생성 후 첫번째 댓글 생성, 참조

    ```sqlite
    article = Article.objects.create(title='title', content='content')
    article.pk
    comment.content = 'first comment'
    comment.article = article
    comment.save()
    comment.pk
    ```

    - 주의사항! - 인스턴스를 어떻게 하느냐에 따라서 참조형식이 달라진다.(권장은 두번째 방식)

      ```sqlite
      comment.article_id = article.pk
      comment.article = article
      ```

  - 첫번째 댓글의 속성값 확인하기

    ```sqlite
    comment.content
    comment.article
    comment.article.pk
    comment.article.content
    ```

  - 두번째 댓글 작성하고 속성값 확인하기

    ```sqlite
    comment = Comment(content='second comment', article=article)
    comment.save()
    comment.pk
    comment.article
    comment.article.pk
    comment.article_id
    ```

    - 주의사항! 이건 안됨

      ```sqlite
      comment.aricle_pk 
      ```

  - admin site에서 작성된 댓글 확인하기

    ```python
    # articles / admin.py
    
    from .models import Aritcle, Comment
    
    admin.site.register(Comment)
    ```

    ```bash
    $ python manage.py createsuperuser
    ```



#### 1:N 관계 related manager

**역참조 ('comment_set')**

- Article(1) -> Comment(N)
- `article.comment` 형태로는 사용할 수 없고, `article.comment_set manger`가 생성됨
- 게시글에 몇 개의 댓글이 작성되었는지 Django ORM이 보장할 수 없기 때문
  - article은 comment가 있을 수도 있고, 없을 수도 있음
  - 실제로 Article 클래스에는 Comment와의 어떠한 관계도 작성되어 있지 않음



**참조('article')**

- Comment(N) -> Article(1)
- 댓글의 경우 어떻나 댓글이든 반드시 자신이 참조하고 있는 게시글이 있으므로 `comment.article`과 같이 접근할 수 있음
- 실제 ForeignKeyField 또한 Comment 클래스에서 작성됨



- 1:N 관계 related manager 연습하기

  - 쉘플러스 실행

    ```bash
    $ python manage.py shell_plus
    ```

  - 역참조할 게시글 조회

    ```sqlite
    article = Article.objects.get(pk=2)
    article
    ```

  - 조회한 인스턴스가 쓸 수 있는 모든 메서드 확인하기

    ```sqlite
    dir(article)
    ```

  - article의 입장에서 모든 댓글 역참조하기

    ```sqlite
    article.comment_set.all()
    ```

  - 조회한 모든 댓글 출력하기

    ```sqlite
    comments = article.comment_set.all()
    for comment in comments:
    	print(comment.content)
    ```

  - [참고] comment_set 명령어를 comments로 변경하기

    - 변경 후에는 기존의 명령어는 사용불가!
    - 변경 후에는 migrate하기!
    - 1:N에서 명령어 변경은 권장하지 않음

    ```python
    class Comment(models.Model):
        article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments')
    ```



### Comment Create

---

#### CommentForm 작성

- `forms.py`작성하기 

  ```python
  # article / forms.py
  from .models import Comment
  
  
  class CommentForm(forms.ModelForm):
  
      class Meta:
          model = Comment
          fields = ('content',)
          # '__all__'을 하면 각 디테일 화면에서도 다른 게시글에 댓글 작성이 가능해짐
  ```

- `views.py` 작성하기

  ```python
  # article / views.py
  from .forms import CommentForm
  
  
  @require_safe
  def detail(request, pk):
      article = get_object_or_404(Article, pk=pk)
      comment_form = CommentForm()
      context = {
          'article': article,
          'comment_form': comment_form
      }
      return render(request, 'articles/detail.html', context)
  ```

- `detail.html`작성하기

  ```django
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>DETAIL</h1>
    <h3>{{ article.pk }}번째 글</h3>
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
    <hr>
    <form action="" method="POST">
      {% csrf_token %}
      {{ comment_form }}
      <input type="submit">
    </form>
  {% endblock content %}
  ```

- 댓글을 제출할 수 있는 url 생성

  ```python
  # articles / urls.py
  
  from django.urls import path
  from . import views
  
  
  app_name = 'articles'
  urlpatterns = [
      path('', views.index, name='index'),
      path('create/', views.create, name='create'),
      path('<int:pk>/', views.detail, name='detail'),
      path('<int:pk>/delete/', views.delete, name='delete'),
      path('<int:pk>/update/', views.update, name='update'),
      path('<int:pk>/comments/', views.comments_create, name='comments_create'),
  ]
  
  ```

- `views.py`에 댓글 등록 함수 작성

  - 댓글은 단독 페이지가 없으므로 if-else 구문 없이 그냥 DB로 받기만 하면 된다.

  ```python
  def comments_create(request, pk):
      article = Article.objects.get(pk=pk)
      comment_form = CommentForm(request.POST)
      if comment_form.is_valid():
          comment = comment_form.save(commit=False)
          comment.article = article
          comment.save()
      return redirect('articles:datial', article.pk)
  ```

  - 주의사항! - 장고에 제출할 때 `commit=False`로 comment.article_id를 같이 제출해주지 않으면 오류발생
    - commit=Flase: 인스턴스는 생성하지만 DB에는 저장하지 않음으로써 인스턴스 추가의 기회를 준다.

- `detail.html` 작성

  ```django
  <form action="{% url 'article:comments_create' article.pk %}" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit">
  </form>
  ```

  

**The 'save()' method**

- **save(commit=False)**
  - Create, but don't save the new instance
  - 아직 데이터베이스에 저장되지 않은 인스턴스를 반환
  - 저장하기 전에 객체에 대한 사용자 지정 처리를 수행할 때 유용하게 사용



### Comment Read

---

#### 댓글 출력

- 특정 article에 있는 몯느 댓글을 가져온 후에 context에 추가

  ```python
  # articles / views.py
  
  form .models import Article, Comment
  
  
  @require_safe
  def detail(request, pk):
      article = get_object_or_404(Article, pk=pk)
      comment_form = CommentForm()
      comments = article.comment_set.all()
      context = {
          'article': article,
          'comment_form': comment_form,
          'comments': comments,
      }
      return render(request, 'articles/detail.html', context)
  ```

- `detail.html`수정

  ```django
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>DETAIL</h1>
    <h3>{{ article.pk }}번째 글</h3>
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
    <hr>
    <h4>댓글 목록</h4>
    <ul>
      {% for comment in comments %} 
        <li>{{ comment.content }}</li>
      {% endfor %}
    </ul>
    <form action="{% url 'article:comments_create' article.pk %}" method="POST">
      {% csrf_token %}
      {{ comment_form }}
      <input type="submit">
    </form>
  {% endblock content %}
  ```

  

### Comment Delete

---

#### 댓글 삭제

- `urls.py`작성하기

  ```python
  # articles / urls.py
  
  from django.urls import path
  from . import views
  
  
  app_name = 'articles'
  urlpatterns = [
      path('', views.index, name='index'),
      path('create/', views.create, name='create'),
      path('<int:pk>/', views.detail, name='detail'),
      path('<int:pk>/delete/', views.delete, name='delete'),
      path('<int:pk>/update/', views.update, name='update'),
      path('<int:pk>/comments/', views.comments_create, name='comments_create'),
      path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
  ]
  ```

- `views.py`작성하기

  ```python
  # articles / views.py
  
  from .models import Article, Comment
  
  def comments_delete(request, article_pk, comment_pk):
      comment = Comment.object.get(pk=comment_pk)
      comment.delete()
      return redirect('articles:detail', article_pk)
  ```

- `detail.html`수정하기

  ```django
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>DETAIL</h1>
    <h3>{{ article.pk }}번째 글</h3>
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
    <hr>
    <h4>댓글 목록</h4>
    <ul>
      {% for comment in comments %}
        <li>
          {{ comment.content }}
          <form action="{% url 'article:comment_delete' article.pk comment.pk %}", method="POST">
            {% csrf_token %}
            <input type="submit" value="삭제">
          </form>
        </li>
      {% endfor %}
    </ul>
    <form action="{% url 'article:comments_create' article.pk %}" method="POST">
      {% csrf_token %}
      {{ comment_form }}
      <input type="submit">
    </form>
  {% endblock content %}
  
  ```

  

#### 인증된 사용자만 생성 및 삭제

- `views.py`수정하기

  ```python
  @require_POST
  def comments_create(request, pk):
      if request.user.is_authenticated:
          article = get_object_or_404(Article, pk=pk)
          comment_form = CommentForm(request.POST)
          if comment_form.is_valid():
              comment = comment_form.save(commit=False)
              comment.article = article
              comment.save()
          return redirect('articles:datail', article.pk)
      return redirect('accounts:login')
  
  
  @require_POST
  def comments_delete(request, article_pk, comment_pk):
      if request.user.is_authenticated:
          comment = get_object_or_404(Comment, pk=comment_pk)
          comment.delete()
          return redirect('articles:detail', article_pk)
  ```



#### Comment 추가사항

- 댓글 개수 출력하기

  ```python
  # 1. {{ comments|length }}
  # 2. {{ article.comments_set.all|length }}
  # 3. {{ comments.count }}
  ```

  ```django
  <!-- articles / detail.html -->
  
  <h4>댓글 목록</h4>
    {% if comments %}
      <p><b>{{ comments|length }}개의 댓글이 있습니다.</b></p>	
    {% endif %}
  ```

- 댓글이 없는 경우 대체 컨텐츠 출력 (DTL의 for-empty태그 활용)



## Customiazing authentication in Django

### Substitutitng a custom User Model

---

#### User 모델 대체하기

- 일부 프로젝트에서는 Django의 내장 User 모델이 제공하는 인증 요구사항이 적절하지 않을 수 있음
  - 예: username 대신 email을 식별 토큰으로 사용하는 것이  더 적합한 사이트
- Django는 User를 참조하는데 사용하는 AUTH_USER_MODEL 값을 제공하여, default user model을 재정의할 수 있도록 함
- Django는 새 프로젝트를 싲가하는 경우 기본 사용자 모델이 충분하더라도, 커스텀 유저 모델을 설정하는 것을 강력하게 권장
  - 단, 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 함



#### AUTH_USER_MODEL

- User를 나타내는데 사용되는 모델
- 프로젝트가 진행되는 동안 변경할 수 없음 (엄밀히 불가능은 아님)
- 프로젝트 시작 시 설정하기 위한 것이며, 참조하는 모델은 첫번째 마이그레이션에서 사용할 수 있어야함
- 기본값: 'auth.User' (auth앱의 User 모델)
- [참고] 프로젝트 중간에 AUTH_USER_MODEL 변경하기
  - 모델 관계에 영향을 미치기 때문에 훨씬 더 어려운 작업이 필요
  - 즉, 중간변경은 권장하지 않으므로 초기에 설정하는 것을 권장



#### Custom User 모델 정의하기

- 관리자 권한과 함께 완전한 기능을 갖춘 User 모델을 구현하는 기본 클래스인 AbstractUser를 상속받아 새로운 User 모델 작성

  ```python
  # accounts / models.py
  
  from django.contrib.auth.models import AbstractUser
  
  
  class User(AbstractUser):
      pass
  ```

- 기존에 DJango가 사용하는 User 모델이었던 auth앱의 User 모델을 accounts앱의 User 모델을 사용하도록`Settings.py`의 `AUTH_USER_MODEL`설정

  ```python
  # crud / settings.py
  
  AUTH_USER_MODEL = 'accounts.User'
  ```

- admin site에 Custom User 모델 등록

  ```python
  # accounts / admin.py
  
  from django.contrib import admin
  from django.contrib.auth.admin import UserAdmin
  from .models import User
  
  
  admin.site.register(User, UserAdmin)
  ```

- 프로젝트 중간에 진행했을 경우 데이터베이스를 초기화한후 마이그레이션 진행

  - 초기화 방법
    1. `db.sqlite3`파일 삭제
    2. `migrations`파일 모두 삭제(파일명에 숫자가 붙은 파일 삭제)

  ```bash
  $ python manage.py makemigrations
  $ python manage.py migrate
  ```



### Custom user & Built-in auth forms

---

#### 회원가입 시도 후 아래와 같은 에러가 발생

- `AttributeError at /accounts/signup/`

  - 회원가입에서 사용한 UserCreationForm은 기존 내장 User 모델을 사용한 ModelForm이기 때문
  - **'`UserCreationForm`'과 '`UserChangeForm`'은 커스텀 User 모델로 대체해야 한다.**
  - **User는 장고에서 직접 참조하지 않고 get_user_model을 통해서 현재 메인으로 활성화 된 모델을 참조한다!**

  1. **대체**

  ```python
  # accounts / forms.py
  
  from django.contrib.auth.forms import UserChangeForm, UserCreationForm
  from django.contrib.auth import get_user_model
  
  
  class CustomUserChangeForm(UserChangeForm):
  
      # password = None
  
      class Meta:
          model = get_user_model() # User
          fields = ('email', 'first_name', 'last_name',)
  
  
  class CustomUserCreationForm(UserCreationForm):
  
      class Meta:
          model = get_user_model()
  ```

  2. **확장**

  ```python
  # accounts / forms.py
  
  from django.contrib.auth.forms import UserChangeForm, UserCreationForm
  from django.contrib.auth import get_user_model
  
  
  class CustomUserChangeForm(UserChangeForm):
  
      # password = None
  
      class Meta:
          model = get_user_model() # User
          fields = ('email', 'first_name', 'last_name',)
  
  
  class CustomUserCreationForm(UserCreationForm):
  
      class Meta(UserCreationForm.Meta):
          model = get_user_model()
          fields = UserCreationForm.Meta.fields + ('email',)
  ```

  

#### get_user_model()

- 현재 프로젝트에서 활성화된 사용자 모델(active user model)을 반환
  - User 모델을 커스터마이징한 상황에서는 Custom User 모델을 반환
- 이 때문에 Django는 User 클래스를 직접 참조하는 대신 `django.contrib.auth.get_user_model()`을 사용해 참조해야 한다.



## 1 : N 관계 설정

### User - Article (1:N)

---

#### CREATE

1. **User와 Article 간 모델 관계 정의 후 migration**

   ```python
   # articles / models.py
   
   from django.db import models
   from django.contrib import settings
   
   
   class Article(models.Model):
       user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
       title = models.CharField(max_length=10)
       content = models.TextField()
       created_at = models.DateTimeField(auto_now_add=True)
       updated_at = models.DateTimeField(auto_now=True)
   
       def __str__(self):
           return self.title
   ```

   - **User 모델 참조하기**

     1. **settings.AUTH_USER_MODEL**
        - User 모델에 대한 외래 키 또는 다대다 관계를 정의할 때 사용해야 함
        - **models.py에서 User 모델을 참조할 때 사용**
     2. **get_user_model()**
        - 현재 활성화(active)된 User 모델을 반환
          - 커스터마이징한 User 모델이 있을 경우는 Custom User 모델, 그렇지 않으면 User를 반환
        - **models.py가 아닌 다른 모든 곳에서 유저 모델을 참조할 때 사용**

     - 둘의 차이 (이건 참고만 하고 결과의 차이만 외우면 된다.)
       - `settings.AUTH_USER_MODEL`의 리턴값은 문자열이고,
       - `get_user_model()`의 리턴값은 오브젝트이다.

   ```bash
   $ python manage.py makemigrations
   $ python manage.py migrate
   ```

   - User와 Article 간 모델 관계 정의 후 migration
     - null 값이 허용되지 않는 user_id 필드가 별도의 값 없이 article에 추가되려 하기 때문에 질문이 뜸
     - 1을 입력 후 enter
       - '현재 화면에서 기본 값을 설정하겠다'
     - 1을 입력 후 enter
       - '기존 테이블에 추가되는 user_id필드의 값을 1로 설정하겠다'

2. **게시글 출력시 작성 페이지에서 불필요한 필드가 출력되므로 제외 시켜준다.**

   ```python
   # articles / forms.py
   
   from django import forms
   from .models import Article, Comment
   
   
   class ArticleForm(forms.ModelForm):
   
       class Meta:
           model = Article
           exclude = ('user',)
   ```

3. **게시글 작성 시 user_id 즉 게시글 작성자 정보(article.user)가 누락되었으므로 추가 해줘야한다.**

   ```python
   # articles / views.py
   
   @login_required
   @require_http_methods(['GET', 'POST'])
   def create(request):
       if request.method == 'POST':
           form = ArticleForm(request.POST)
           if form.is_valid():
               article = form.save(commit=False)
               article.user = request.user
               article.save()
               return redirect('articles:detail', article.pk)
       else:
           form = ArticleForm()
       context = {
           'form': form,
       }
       return render(request, 'articles/create.html', context)
   ```

   

#### DELETE

- 자신이 작성한 게시글만 삭제 가능하도록 설정

  ```python
  # articles / views.py
  
  @require_POST
  def delete(request, pk):
      if request.user.is_authticated:
          article = get_object_or_404(Article, pk=pk)
          if request.user == article.user:
              article.delete()
      return redirect('articles:index')
  ```



#### UPDATE

- 자신이 작성한 게시글만 수정 가능하도록 설정

  ```python
  # articles / views.py
  
  @login_required
  @require_http_methods(['GET', 'POST'])
  def update(request, pk):
      article = get_object_or_404(Article, pk=pk)
      if request.user == article.user:
          if request.method == 'POST':
              form = ArticleForm(request.POST, instance=article)
              if form.is_valid():
                  article = form.save()
                  return redirect('articles:detail', article.pk)
          else:
              form = ArticleForm(instance=article)
      else:
          return redirect('articles:index')
      context = {
          'article': article,
          'form': form,
      }
      return render(request, 'articles/update.html', context)
  ```



#### [참고] READ

- 게시글 작성 user가 누구인지 index.html에서 출력하기

  ```django
  <!-- articles / index.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>Articles</h1>
    {% if request.user.is_authenticated %}
      <a href="{% url 'articles:create' %}">CREATE</a>
    {% else %}
      <a href="{% url 'accounts:login' %}">[새 글을 작성하려면 로그인 하세요]</a>
    {% endif %}
    <hr>
    {% for article in articles %}
      <p><b>작성자: {{ article.user }}</b></p>
      <p>글 번호: {{ article.pk }}</p>  
      <p>글 제목: {{ article.title }}</p>
      <p>글 내용: {{ article.content }}</p>
      <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
      <hr>
    {% endfor %}
  {% endblock content %}
  ```

- 아예 작성자가 아니라면 수정 삭제 버튼을 출력하지 않기

  ```django
  <!-- articles / detail.html -->
  
  {% if request.user == article.user %}
    <a href="{% url 'articles:update' article.pk %}">수정</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="삭제">
    </form>
  {% endif %}
  ```



### User - Comment (1:N)

---

#### CREATE

1. **User와 Comment 간 모델 관계 정의 후 migration**

   ```python
   # articles / models.py
   
   from django.db import models
   from django.contrib import settings
   
   
   class Comment(models.Model):
       article = models.ForeignKey(Article, on_delete=models.CASCADE)
       user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
       content = models.CharField(max_length=200)
       created_at = models.DateTimeField(auto_now_add=True)
       updated_at = models.DateTimeField(auto_now=True)
       
       def __str__(self):
           return self.content
   ```

   ```bash
   $ python manage.py makemigrations
   $ python manage.py migrate
   ```

2. **게시글 작성 시 user_id 즉 게시글 작성자 정보(comment.user)가 누락되었으므로 추가 해줘야한다.**

   ```python
   # articles / views.py
   
   @require_POST
   def comments_create(request, pk):
       if request.user.is_authenticated:
           article = get_object_or_404(Article, pk=pk)
           comment_form = CommentForm(request.POST)
           if comment_form.is_valid():
               comment = comment_form.save(commit=False)
               comment.article = article
               comment.user = request.user
               comment.save()
           return redirect('articles:datail', article.pk)
       return redirect('accounts:login')
   ```



#### READ

- 비로그인 유저에게는 댓글 form 출력 숨기기

  ```django
  <!-- articles / detail.html -->
  
  {% if request.user.is_authenticated %}
    <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
      {% csrf_token %}
      {{ comment_form }}
    <input type="submit">
    </form>
  {% else %}
    <a href={% url 'accounts.login' %}>[댓글을 작성하려면 로그인하세요.]</a>
  {% endif %}
  ```

- 댓글 작성자 출력하기

  ```django
  <!-- articles / detail.html -->
  
  <ul>
      {% for comment in comments %}
        <li>
          {{ comment.user}} - {{ comment.content }}
          <form action="{% url 'articles:comments_delete' article.pk comment.pk %}", method="POST">
            {% csrf_token %}
            <input type="submit" value="삭제">
          </form>
        </li>
      {% endfor %}
    </ul>
  ```

  

#### DELETE

- 자신이 작성한 댓글에만 삭제가 보이고 삭제 가능하도록 설정

  ```python
  # articles / views.py
  
  @require_POST
  def comments_delete(request, article_pk, comment_pk):
      if request.user.is_authenticated:
          comment = get_object_or_404(Comment, pk=comment_pk)
          if request.user == comment.user:
              comment.delete()
      return redirect('articles:detail', article_pk)
  ```

  ```django
  <!-- articles / detail.html-->
  
  {% if user == comment.user %}
    <form action="{% url 'articles:comments_delete' article.pk comment.pk %}", method="POST">
      {% csrf_token %}
      <input type="submit" value="삭제">
    </form>
  {% endif %}
  ```

