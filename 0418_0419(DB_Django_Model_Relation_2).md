

# DataBase - Model Relationshipe(M:N)

## Intro

### 병원 진료 기록 시스템

---

#### 병원 진료 기록 시스템을 통한 M:N 관계 학습

**환자와 의사가 사용하는 병원 진료 기록 시스템 구축**

- 병원 시스템에서 가장 핵심이 되는 객체 => 환자와 의사



**시작하기 전**

- **모델링은 현실 세계를 최대한 유사하게 반영하기 위한 것**
- 우리 일상에 가까운 예실르 통해 DB를 모델링하고, 그 내부에서 일어나는 데이터의 흐름을 어떻게 제어할 수 있을지 고민해보기



#### **1:N 관계의 한계**

---

```python
from django.db import models


class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- 의사 - 환자의 관계가 1:N인 경우,

- 1번 환자가 1번 의사의 진료를 마치고, 2번 의사에게도 방문하려고 한다면 새로운 예약을 생성해야 한다.
- 기존의 예약을 유지한 상태로 새로운 예약을 생성
- 새로 생성한 3번 환자(tony)는 1번 환자(tony)와 다르다. (실제 세계에서는 같지만)
- 한 번에 두 의사에게 진료를 받고자 하는 경우, 하나의 외래 키에 2개의 의사 데이터를 넣을 수도 없다.



- **새로운 객체를 생성하지 않고 기존의 객체에 대해서 새로운 예약을 생성하는 것이 불가능하다.**
- **여러 의사에게 진료 받은 기록을 환자 한명에 저장할 수 없다.**



#### **중개 모델** 

---

```python
from django.db import models


class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


# 외래키 삭제
class Patient(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

# 중개모델 작성
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
```

- 중개 모델(혹은 중개 테이블, Associative Table) 작성
- `Doctor`와도 1:N 관계이고 `Patient`와도 1:N 관계에 있는 `Reservation`클래스 작성
- 서로가 서로를 역참조



#### **ManyToManyField**

---

```python
from django.db import models


class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    # ManyToManyField 작성
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- 다대다(M:N, many - to -many) 관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스)가 필요
- **ManyToManyField 작성 (중개 모델 삭제)**
  - **필드 작성 위치는 Doctor 또는 Patient 모두 작성 가능**
  - **서로가 서로를 역참조하던 중개 테이블과는 다르게 필드가 작성된 테이블이 상대 테이블을 참조하고 나머지 필드가 역참조를 한다.**



- **참조**

  ```python
  doctor1 = Doctor.objects.create(name='justin')
  patient1 = Patient.objects.create(name='tony')
  patient2 = Patient.objects.create(name='harry')
  
  # 새로운 의사를 더해주기
  patient1.doctors.add(doctor1)
  patient1.doctors.all()
  doctor1.patient_set.all()
  
  # 새로운 환자를 더해주기
  doctor1.patient_set.add(patient2)
  doctor1.patient_set.all()
  patient2.doctors.all()
  patient1.doctors.all()
  
  # 둘 중 누가 비우더라도 상관없음
  # 환자를 비우기
  doctor1.patient_set.remove(patient1)
  doctor1.patient_set.all()
  patient1.doctors.all()
  
  # 의사를 비우기
  patient2.patient_set.remove(doctor1)
  patient2.doctors.all()
  doctor1.patient_set.all()
  ```



- **related_name**

  ```python
  class Patient(models.Model):
      # ManyToManyField - related_name 작성
      doctors = models.ManyToManyField(Doctor, related_name='patients')
      name = models.TextField()
  ```

  - target model(관계필드를 가지지 않은 모델)이 source model(관계 필드를 가진 모델)을 참조할 때 사용할 manager의 이름을 설정
  - 즉,  역참조 시에 사용하는 manager의 이름을 설정
  - ForeignKey의 related_name과 동일
  - related_name 설정 후 기존의 _set manager는 더 이상 사용할 수 없다.



- Django의 중개 모델(테이블)
  - django는 ManyToManyField를 통해 중개 테이블을 자동으로 생성
  - 그렇다면 중개 테이블을 직접 작성하는 경우는 없을까?
    - 중개 테이블을 수동으로 지정하려는 경우 through옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정할 수 있음
    - 가장 일반적인 용도는 중개 테이블에 추가 데이터를 사용해 다대다 관계로 연결하려는 경우에 사용



- 요약
  - 실제 Doctor와 Patient 테이블이 변하는 것은 없음
  - 1:N 관계는 완전한 종속의 관계이지만 M:N 관계는 의사에게 진찰받는 환자, 호나자를 진찰하는 의사의 두가지 형태로 모두 표현이 가능한 것



#### ManyToManyField + 중개모델

---

```python
class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation')
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

```python
doctor1 = Doctor.objects.create(name='justin')
patient1 = Patient.objects.create(name='tony')
patient2 = Patient.objects.create(name='harry')

reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
reservation1.save()
doctor1.patient_set.all()
patient1.doctors.all()

# through 사용한 예약도 가능
patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
doctor1.patient_set.all()
patient2.doctors.all()

doctor1.patient_set.remove(patient1)
patient2.doctors.remove(doctor1)
```



## ManyToMany Field

### ManyToManyField's 개념 및 특징

---

- 다대다 관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자(다대가 관계로 설정할 모델 클래스)가 필요
- 모델 필드의 RelatedManager(일대다 또는 다대다 관련 컨텍스트에서 사용되는 manager)를 사용하여 관련 개체를 추가,  제거 또는 만들 수 있음
  - add(), remove()



- **ManyToManyField's Arguments**
  1. **related_name**
     - **targer model이 source model을 참조할 때 사용할 manager의 이름을 설정**
     - ForeighKey의 related_name과 동일
  2. **through**
     - **중개 테이블을 직접 작성하는 경우,  through 옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정**할 수 있다.
     - 일반적으로 중개 테이블에 추가 데이터를 사용하는 다대다 관계와 연결하려는 경우에 주로 사용된다.
  3. **symmetrical**
     - **ManyToMayField가 동일한 모델(on self)을 가리키는 정의에서만 사용**
     - symmetrical=True(기본값)일 경우 Django는 person_set 매니저를 추가하지 않음
     - source 모델의 인스턴스가 target모델의 인스턴스를 참조하면 target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함



- **Related Manager**

  - **1:N 또는 M:N 관련 컨텍스트에서 사용되는 매니저**
  - 같은 이름의 메서드여도 각 관계(1:N, M:N)에 따라 다르게 사용 및 동작
    - 1:N에서는 target 모델 인스턴스만 사용 가능
    - M:N 관계에서는 관련된 두 객체에서 모두 사용 가능

  - 메서드 종류
    - **add()**
      - '지정된 객체를 관련 객체 집합에 추가'
      - 이미 존재하는 관계에 사용하면 관계가 복제되지 않음
      - 모델 인스턴스, 필드 값(PK)을 인자로 허용
    - **remove()**
      - '관련 객체 집합에서 지정된 모델 객체를 제거'
      - 내부적으로 QuerySet.delete()를 사용하여 관계가 삭제됨
      - 모델 인스턴스, 필드 값을 인자로 허용



- **데이터베이스에서의 표현**
  - Django는 다대다 관계를 나타내는 중개 테이블을 만듦
  - 테이블 이름은 다대다 필드의 이름과 이를 포함하는 모델의 테이블 이름을 조합하여 생성됨



- **중개 테이블의 필드 생성 규칙**
  1. source model 및 target model 모델이 다른 경우
     - `id`
     - `<containing_model>_id`
     - `<other_model>_id`
  2. ManyToManyField가 동일한 모델을 가리키는 경우
     - `id`
     - `from_<model>_id`
     - `to_<model>_id`



### 좋아요 기능(Like)

---

#### Like 구현하기

- **model 작성: 서로 다대다 관계에 있는 모델 중 어떤 곳이든 상관없이 관계 생성**

  ```python
  # articles / models.py
  
  class Article(models.Model):
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
      like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
      title = models.CharField(max_length=10)
      content = models.TextField()
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  
      def __str__(self):
          return self.title
  ```

  - [주의사항] **에러 발생 원인**
    - `like_users`필드 생성 시 자동으로 역참조는 `.articles_set`매니저를 생성
    - 그러나 이전 1:N 관계에서 이미 해당 매니저 이름을 사용하고 있어 충돌이 발생한다.
    - 그러므로 **User와 관계된 ForeignKey 또는 ManyToManyField 중 하나에 related_name 추가**가 필요하다.



- **현재 User - Article 간 사용 가능한 DB API**
  - article.user : 게시글을 작성한 유저 (일대다)
  - article.like_users : 게시글을 좋아요한 유저 (다대다)
  - user.article_set : 유저가 작성한 게시글 (일대다 역참조)
  - user.like_articles : 유저가 좋아요한 게시글 (다대다 역참조)



- **url 작성**

  ```python
  # articles / urls.py
  
  from django.urls import path
  from . import views
  
  app_name = 'articles'
  urlpatterns = [
      ... ,
      path('<int:articles_pk>/likes/', views.likes, name='likes'),
  ]
  ```



- **view 함수 작성**

  ```python
  # articles / views.py
  
  def likes(request, article_pk):
      article = get_object_or_404(Article, pk=article_pk)
  
      # 이 게시글에 좋아요를 누른 유저 목록에 현재 요청하는 유저가 있다면 좋아요 취소
      if request.user in article.like_users.all():
          article.like_users.remove(request.user)
      # 아니면 좋아요를 누른 유저 목록에 현재 요청하는 유저를 추가
      else:
          article.like_users.add(request.user)
      return redirect('articles:index')
  ```

  ```python
  def likes(request, article_pk):
  	if article.like_users.filter(pk=request.user.pk).exists():
          article.like_users.remove(request.user)
      else:
          article.like_users.add(request.user)
      return redirect('articles:index')
  ```

  - QuerySet API - '`exists()`'
    - QuerySet에 결과가 포함되어 있으면 True를 반환하고 그렇지 않으면 False를 반환
    - 특히 규모가 큰 QuerySet의 컨텍스트에서 특정 개체 존재여부와 관련된 검색에 유용
    - 고유한 필드가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법

​	

- **index.html 수정**

  ```django
  <div>
    <form action="{% url 'articles:likes' article.pk %}" method="POST">
      {% csrf_token %}
      {% if user in articles.like_users.all %}
        <input type="submit" value="좋아요 취소">
      {% else %}
        <input type="submit" value="좋아요"
      {% endif %}
    </form>
  </div>
  ```



### Profile Page

---

- **url 작성**

  ```python
  # accounts / urls.py
  
  urlpatterns = [
      path('<username>/', views.profile, name='profile'),
  ]
  ```

  - **[주의사항]** `<문자열>` 형태의 variable_routing을 사용할 때는 urlpatterns의 가장 마지막에 작성해야만 한다!



- **view 함수 작성**

  ```python
  # accounts / views.py
  
  from django.shortcuts import render, redirect, get_object_or_404
  from django.contrib.auth import get_user_model
  
  
  def profile(request, username):
      User = get_user_model
      person = get_object_or_404(User, username=username)
      context = {
          'person': person,
      }
      return render(request, 'accounts/profile.html', context)
  ```



- profile.html 생성

  ```django
  {% extends 'base.html' %}
  
  {% block content %}
  <h1>{{ person.username }}님의 프로필</h1>
  
  <hr>
  
  {% comment %} 이 사람이 작성한 게시글 목록 {% endcomment %}
  <h2>{{ person.username }}이 작성한 게시글</h2>
  {% for article in person.article_set.all  %}
    <p>{{ articles.title }}</p>
  {% endfor %}
  
  <hr>
  
  {% comment %} 이 사람이 작성한 댓글 목록 {% endcomment %}
  <h2>{{ person.username }}이 작성한 댓글</h2>
  {% for comment in person.comment_set.all  %}
    <p>{{ comment.content }}</p>
  {% endfor %}
  
  <hr>
  
  {% comment %} 이 사람이 좋아요를 누른 게시글 목록 {% endcomment %}
  <h2>{{ person.username }}이 좋아요를 누른 게시글</h2>
  {% for article in person.like_articles.all %}
    <p>{{ article.title }}</p>
  {% endfor %}
  
  {% endblock content %}
  ```



### 팔로우 기능 (Follow)

---

### ManyToManyField's Arguments

---

#### symmetrical

- ManyToManyField가 동일한 모델(on self)을 가리키는 정의에서만 사용
- symmetrical=True(기본값)일 경우 Django는 person_set 매니저를 추가하지 않음
  - 자동으로 동시에 서로 참조
- source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면, target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함
  - 즉,  내가 당신의 친구라면 당신도 내 친구가 되는 것
  - 대칭을 원하지 않는 경우 False로 설정



#### Follow 기능 구현

- **model 작성 / migrate**

  ```python
  # accounts / models.py
  
  from django.db import models
  from django.contrib.auth.models import AbstractUser
  
  
  class User(AbstractUser):
      followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
  ```

  ```bash
  $ python manage.py makemigrations
  $ python manage.py migrate
  ```

  

- **url 작성**

  ```python
  # accounts / urls.py
  
  from django.urls import path
  from . import views
  
  
  app_name = 'accounts'
  urlpatterns = [
      ... ,
      path('<int:user_pk>/follow/', views.follow, name='follow'),
  ]
  ```



- **view 함수 작성**

  ```python
  def follow(request, user_pk):
      you = get_object_or_404(get_user_model(), pk=user_pk)
      me = request.user
  
      if me != you:
          if me in you.followers.all():
              you.followers.remove(me)
          else:
              you.followers.add(me)
      return redirect('accounts:profile', you.username)
  ```

  

- **profile.html 작성**

  ```django
  <h1>{{ person.username }}님의 프로필</h1>
  <div>
    팔로워 : {{ person.followers.all|length }} / 팔로우 : {{ person.followings.all|length }}
  </div>
  
  <div>
    {% if user != person %}
      <form action="{% url 'accounts:follow' %}" method='POST'>
        {% csrf_token %}
        {% if user in person.followers.all %}
          <input type="submit" value='언팔로우'>
        {% else %}
          <input type="submit" value='팔로우'>
        {% endif %}
      </form>
    {% endif %}
  </div>
  ```

  - Tip! - with 태그를 사용하여 자주쓰는 변수를 간단하게!

    ```django
    {% with followers=person.followers.all followings=person.followings.all %}
      <div>
        팔로워 : {{ followers|length }} / 팔로우 : {{ followings|length }}
      </div>
    
      <hr>
    
      <div>
        {% if user != person %}
          <form action="{% url 'accounts:follow' %}" method='POST'>
            {% csrf_token %}
            {% if user in followers %}
              <input type="submit" value='언팔로우'>
            {% else %}
              <input type="submit" value='팔로우'>
            {% endif %}
          </form>
        {% endif %}
      </div>
    {% endwith %}
    ```

    
