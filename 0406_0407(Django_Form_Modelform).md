# Django: Form / Model Form

## Form Class

- 우리는 지금까지 HTML form, input을 통해서 사용자로부터 데이터를 받음
- 이렇게 직접 사용자의 데이터를 받으면 입력된 데이터의 유효성을 검증하고 필요시에 입력된 데이터를 검증결과와 함께 다시 표시해야함
  - 사용자가 입력한 데이터는 개발자가 요구한 형식이 아닐 수 있음을 항상 생각해야함
- 사용자가 입력한 데이터를 검증하는 것을 '유효성 검증'이라고 하는데, 이 과정을 코드로 모두 구현하는 것은 많은 노력이 필요한 작업이다.
- Django는 이러한 과중한 작업과 반복 코드를 줄여줌으로써 이 작업을 훨씬 쉽게 만들어주는데 이것이 바로 **'Django Form'**이다.



**Django's forms**

---

- **Form은 Django의 유효성 검사 도구 중 하나로 외부의 악의적 공격 및 데이터 손상에 대한 중요한 방어수단**이다.
- Django는 Form과 관련한 유효성 검사를 단순화하고 자동화 할 수 있는 기능을 제공하여 개발자로 하여금 **직접 작성하는 코드보다 더 안전하고 빠르게 수행하는 코드를 작성**할 수 있게 한다.
- Django는 form에 관련된 작업의 아래 세 부분을 처리해준다.
  1. **렌더링을 위한 데이터 준비 및 재구성**
  2. **데이터에 대한 HTML forms 생성**
  3. **클라이언트로부터 받은 데이터 수신 및 처리**



**The Django 'Form Class'**

---

- **Django Form 관리 시스템의 핵심**
- Form 내 **field, field배치, 디스플레이 위젯, 라벨, 초기값, 유효하지 않는 field에 관련된 에러 메세지를 결정**
- Django는 사용자의 데이터를 받을 때 해야할 과중한 작업(데이터 유효성 검증, 필요시 입력된 데이터 검증결과 재출력, 유효한 데이터에 대해 요구되는 동작 수행 등)과 반복 코드를 줄여준다.



**Form 선언하기**

---

```python
# articles/forms.py

from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length = 10)
    content = forms.CharField()
```

- Model을 선언하는 것과 유사하며 같은 필드 타입을 사용(그러나 완전 일치하지는 않는다.)
- forms 라이브러리에서 파생된 Form 클래스를 상속받음
- **forms.py 파일위치**
  - Form class는 forms.py 뿐만 아니라 다른 어느 위치에 두어도 상관 없다.
  - 하지만 되도록 `app폴더/forms.py`에 작성하는 것이 일반적인 구조!



**Form 사용하기**

---

```python
# articles/views.py

from .forms import ArticlesForm

def new(request):
    form = ArticlesForm()
    context = {
        'form' : form,
    }
    return render(request, 'articles/new.html', context)
```

```html
<!-- articles/templates/articles/new.html -->
{% extends 'base.html' %}


{% block content %}
  <h1>NEW</h1>
  <hr>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```



**Form rendering options**

---

- `<label>` & `<input>` 쌍에 대한 **3가지 출력 옵션**

  1. **as_p()**

     - 각 필드가 **단락(`<p>`태그)으로 감싸져서 렌더링** 됨

     ```html
     <!-- articles/templates/articles/new.html -->
     {% extends 'base.html' %}
     
     
     {% block content %}
       <h1>NEW</h1>
       <hr>
       <form action="{% url 'articles:create' %}" method="POST">
         {% csrf_token %}
         {{ form }}
         <input type="submit">
       </form>
       <a href="{% url 'articles:index' %}">back</a>
     {% endblock content %}
     ```

  2. **as_ul()**

     - 각 필드가 **목록 항목(`<li>`태그)으로 감싸져서 렌더링** 됨
     - `<ul>`태그는 직접 작성해야 함

  3. **as_table()**

     - 각 필드가 **테이블행으로 감싸져서 렌더링** 됨
     - `<table>`태그는 직접 작성해야함



**Django의 HTML input 요소 표현 방법 2가지**

---

1. **Form fields**

   - input에 대한 유효성 검사 로직을 처리하며 템플릿에서 직접 사용된다.

2. **Widgets**

   - 웹페이지의 HTML input 요소 렌더링
   - GET/POST 딕셔너리에서 데이터 추출
   - widgets은 반드시 Form fields에 할당된다(단독적으로 사용할 수는 없음)
   - DJango의 HTML input element 표현
   - HTML 렌더링 처리
   - **주의 사항**
     - Form Fields와 혼동되어서는 안됨
     - Form Fields는 input 유효성 검사를 처리
     - Widgets은 웹페이지에서 input element의 단순 raw한 렌더링 처리
   - 사용예시

   ```python
   # articles/forms.py
   
   from django import forms
   
   class ArticleForm(forms.Form):
       title = forms.CharField(max_length = 10)
       content = forms.CharField(widget=forms.Textarea)
   ```

   - django widget 공식문서 참고!



**Form field 및 widget응용 예시** 

---

```python
# articles/forms.py

from django import forms

class ArticleForm(forms.Form):
    REGION_A = "sl"
    REGION_B = "dj"
    REGION_C = "gj"
    REGION_D = "gm"
    REGION_E = "bu"
    REGIONs_CHOICES = [
        (REGION_A, "서울"),
        (REGION_B, "대전"),
        (REGION_C, "광주"),
        (REGION_D, "구미"),
        (REGION_E, "부산"),
    ]
    
    title = forms.CharField(max_length = 10)
    content = forms.CharField(widget=forms.Textarea)
    region = forms.ChoiceField(choices=REGIONS_CHOICES, widget=forms.Select())
```



## ModelForm

- Django Form을 사용하다보면 Model에 정의한 필드를 유저로부터 입력받기 위해 Form에서 Model 필드를 재정의하는 행위가 중복될 수 있음
- 그래서 Django는 Model을 통해 Form Class를 만들 수 있는 ModelForm이라는 Helper를 제공



**ModelForm Class**

---

- Model을 통해 Form Class를 만들수 있는 Helper
- 일반 Form Class와 완전히 같은 방식(객체 생성)으로 view에서 사용가능



**Form과 ModelForm의 차이**

---

- 대표적인 예로는 **로그인(Form)와 회원가입(ModelForm)**의 차이
  - 사용자로부터 입력받은 데이터를 **단순히 사용을 하느냐**(Form) **DB에 저장이 되느냐**(ModelForm)에 따라서 용도가 나뉜다.
- **Form**
  - 어떤 Model에 저장해야 하는지 알 수 없으므로 유효성 검사 이후 cleaned_data 딕셔너리를 생성
  - cleaned_data 딕셔너리에서 데이터를 가져온 후 .save() 호출해야 함
  - Model에 연관되지 않은 데이터를 받을 때 사용
- **ModelForm**
  - Django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의
  - 어떤 레코드를 만들어야 할 지 알고 있으므로 바로 .save() 호출 가능

 

**ModelForm 선언하기**

---

```python
# articles/forms.py

from django import forms
from .models import Aritcle

class ArticleForm(forms.ModeForm):
    
    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title',)
        # 만약 전체에서 일부를 제외하는 것이 더 편하다면 fields말고 exclude사용
        # fields와 exclude 중 하나만 사용
```

- forms 라이브러리에서 파생된 ModelForm클래스를 상속받음
- 정의한 클래스 안에 Meta클래스를 선언하고, 어떤 모델을 기반으로 Form을 작성할 것인지에 대한 정보를 Meta클래스에 지정
- 주의사항: `class Meta`에서 `model = Article`는 함수로 호출하면 안된다.
  - 호출하면 model이 Article 클래스의 인스턴스로 생성되기 때문!



**Meta class**

---

- **Model의 정보를 작성하는 곳**
- ModelForm을 사용할 경우 사용할 모델이 있어야하는데 Meta Class가 이를 구성함
  - 해당 Model에 정의한 field 정보를 Form에 적용하기 위함



- [참고] **Inner Class(Nested Class)**
  - **클래스 내에 선언된 다른 클래스**
  - 관련 클래스를 함께 그룹화하여 가독성 및 프로그램 유지관리를 지원(논리적으로 묶어서 표현)
  - 외부에서 내부 클래스에 접근할 수 없으므로 **코드의 복잡성을 줄일 수 있음**



- [참고] **Meat 데이터**
  - '데이터에 대한 데이터'



**Create view 함수 수정** 

---

```python
# articles/views.py

def create(request):
    form = ArticleForm(request.POST)
    if form.is_vaild():
        article = form.save()
        return redirect('articles:detail', article.pk)
    return redirect('articles:new')
```

- form.save한 것이 article
- 만약 form의 유효성 검사 결과가 참이라면 'articles:detail'으로 리다이렉트 하고, 거짓이라면 'articles:new'로 리다이렉트 한다.
- 악성 사용자가 html을 수정해도 유효성검사에 걸림 



- **is_valid() method** (유효성 검사)
  - 유효성 검사를 실행하고, 데이터가 유효한지 여부를 boolean으로 반환
  - 데이터 유효성 검사를 보장하기 위한 많은 테스트에 대해 Django는 is_valid()를 제공
  - [참고] 유효성 검사
    - 요청한 데이터가 특정 조건에 충족하는지 확인하는 작업
    - 데이터베이스 각 필드 조건에 올바르지 않은 데이터가 서버로 전송되거나 저장되지 않도록 하는 것



- **The save() method**

  - Form에 바인딩 된 데이터에서 데이터 베이스 객체를 만들고 저장 

  - ModelForm의 하위(sub) 클래스는 기존 모델 인스턴스를 키워드 인자 instance로 받아들일 수 있음

    - 이것이 제공되면 save()는 해당 인스턴스를 수정(UPDATE)
    - 제공되지 않은 경우 save()는 지정된 모델의 새 인스턴스를 만듦(CREATE)

  - Form의 유효성이 확인되지 않은 경우 save()를 호출하면 form.errors를 확인하여 에러 확인 가능

    ```python
    # articles/views.py
    
    def create(request):
        form = ArticleForm(request.POST)
        if form.is_vaild():
            article = form.save()
            return redirect('articles:detail', article.pk)
        print(form.errors)
        return redirect('articles:new')
    ```

  - 예시

    ```python
    # Create a form instance from POST data
    form = ArticleForm(request.POST)
    
    # CREATE
    # Save a new Article object from the form's data.
    # 기존에 없으면 생성으로 판단
    new_article = form.save()
    
    # UPDATE
    # Create a form to edit an existing Article, but use POST data to populate the form.
    # 기존이 있으면 수정으로 판단
    article = Article.objects.get(pk=1)
    form = ArticleForm(request.POST, instance=article)
    form.save()
    ```

    

- **view 함수 구조 변경**

  - 기존의 GET 과 POST를 따로 담당하고 있는 두 view함수를 하나로 합친다.

  ```python
  def create(request):
      if request.method == 'POST':
          # create
          form = ArticleForm(request.POST)
  
          # 유효성검사: 관리자가 정한 기준을 벗어나면 여기서 걸러짐
          if form.is_valid():
              article = form.save()
              return redirect('articles:detail', article.pk)
          
      else:
          # new
          form = ArticleForm()
  
      # context가 받는 form은 
      # 1.ArticleForm()의 인스턴스 form
      # 2.유효성 검사를 통과하지 못한 form 두 가지를 받는다.    
      context = {
          'form' : form,      
      }
      return render(request, 'articles/create.html', context)
  ```

  - 주의사항: context와 render 함수의 위치
  - 왜 if / else문에서 if 문에 'POST'를 확인하는가?
    - 'POST'와  'GET'함수 외에도 많은 메서드가 있지만 그 중 DB에 직접 조작을 가하는 메서드는 'POST'만 있다. 그러므로 POST 외에 다른 함수들이 DB를 조작하는 것을 막기 위해서 if 문에서 POST를 확인하고 else 문에서 POST가 아닌 다른 메서드들을 확인한다.



- **create.html 변경**

  ```html
  <!-- create.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
  	<h1 class='text-center'>CREATE</h1>
  	<form action="{% url 'articles:create' %}" method="POST">
      	{% csrf_token %}    
          {{ form.as_p }}
          <input type="submit">
  	</form>
  	<hr>
  	<a href="{% url 'articles:index' %}">[back]</a>
  {% endblock %}
  ```

  - form에 action의 속성이 없으면 현재 url로 받아들이는데 현재는 create로 둘이 통합된 상태이므로 문제가 생기지 않는다. (권장은 하지 않음)

    ```
    <form action method="POST">
    ```

  ```html
  <!-- index.html -->
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>Articles</h1>
    <a href="{% url 'articles:create' %}">CREATE</a>
    <hr>
    {% for article in articles %}
      <p>글 번호: {{ article.pk }}</p>  
      <p>글 제목: {{ article.title }}</p>
      <p>글 내용: {{ article.content }}</p>
      <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
      <hr>
    {% endfor %}
  {% endblock content %}
  ```



**UPDATE view 함수 수정**

---

- **view 함수 구조 변경**

  - 기존의 GET 과 POST를 따로 담당하고 있는 두 view함수를 하나로 합친다.

  ```python
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      
      if request.mothed == "POST":
          form = ArticleForm(request.POST, instance=article)
  
          # 유효성검사: 관리자가 정한 기준을 벗어나면 여기서 걸러짐
          if form.is_valid():
              article = form.save()
              return redirect('articles:detail', article.pk)
      
      else:
          
          form = ArticleForm(instance=article)
  
      context = {
          'article' : article,
          'form' : form,
      }
      return render(request, 'articles/update.html', context)
  ```

  - form 인스턴스에 instance 속성에 과거의 데이터가 들어가느냐 마느냐에 따라서 create 또는 update가 단락이 나뉨

​	

- **update.html**

  ```html
   {% extends 'base.html' %}
  
  
  {% block content %}
    <h1>UPDATE</h1>
    <hr>
    <form action="{% url 'articles:update' article.pk %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
    <a href="{% url 'articles:detail' article.pk %}">back</a>
  {% endblock content %}
  ```



 **Widgets 활용하기**

---

- **Django의 HTML input element 표현**
- **HTML 렌더링을 처리**
- **2가지 작성 방식을 가짐**



1. **첫번째 방식**: `forms.py`의 Meta 클래스에서 작성

   ```python
   # articles/forms.py
   
   from django import forms
   from .models import Aritcle
   
   class ArticleForm(forms.ModeForm):
       
       class Meta:
           model = Article
           fields = '__all__'
           widjets = {
               'title': forms.TextInput()
           }
   ```

2. **[권장사항]** **두번째 방식**: `forms.py`의 ModelForm 클래스 내부에 Form 클래스처럼 작성

   ```python
   # articles/forms.py
   
   from django import forms
   from .models import Aritcle
   
   class ArticleForm(forms.ModelForm):
       title = forms.CharField(
           widget=forms.TextInput(
   	        attrs={
                   'class': 'my-title',
                   'placeholder': 'Enter the title',
               }
           )
       )
       
       class Meta:
           model = Article
           fields = '__all__'
   ```

   - `create.html`의 `{{ form.as_p }}`와 같은 태그에서는 안에 수정을 가할 수 없기 때문에 ModelForm을 사용할 때는 위젯을 사용해서 속성값을 따로 지정해 줘야한다.

   - **error_messages**도 **widget**처럼! (둘은 동일 레벨에 작성)

     ```python
     class ArticleForm(forms.ModelForm):
         title = forms.CharField(
             widget=forms.TextInput(
     	        attrs={
                     'class': 'my-title',
                     'placeholder': 'Enter the title',
                 }
             )
         )
         content = forms.CharField(
             widget=forms.TextInput(
                 attrs={
                     'class': 'my_content',
                     'placeholder': 'Enter the content'
                 }
             ),
             error_messages={
                 'required': 'Please enter your content!!!',
             }
         )
     ```



**ModelForm 정리**

---

1. 모델 필드 속성에 맞는 html element를 만들어주고,
2. 이를 통해 받은 데이터를 view 함수에서 유효성 검사를 할 수 있도록 함



## Rendering fields manually

**수동으로 Form 작성하기**

---

1. **Rendering fields manually** (각각 따로 지정해야하는 경우)

   ```html
   <!-- articles/create.html -->
   
     <h1>CREATE</h1>
     <hr>
     <form action="{% url 'articles:create' %}" method="POST">
       {% csrf_token %}
       <div>
           {{ form.title.errors }}
           {{ form.title.label_tag }}
           {{ form.title }}
       </div>
       <div>   
           {{ form.content.errors }}
           {{ form.content.label_tag }}
           {{ form.content }}
       </div>
     </form>
     <a href="{% url 'articles:index' %}">back</a>
   {% endblock content %}
   ```

   

2. **Looping over the form's fields** (`{% for %}`)

   ```html
   <!-- articles/create.html -->
   
     <h1>CREATE</h1>
     <hr>
     <form action="{% url 'articles:create' %}" method="POST">
       {% csrf_token %}
       {% for field in form %}
         {{ field.errors }}
         {{ field.label_tag }}
         {{ field }}
       {% endfor %}
     </form>
     <a href="{% url 'articles:index' %}">back</a>
   {% endblock content %}
   ```

   

**Bootstrap과 함께 사용하기**

---

1. **Bootstrap class with widgets**

   ```python
   # forms.py
   
   class ArticleForm(forms.ModelForm):
       title = forms.CharField(
           widget=forms.TextInput(
   	        attrs={
                   'class': 'my-title form-control',
                   'placeholder': 'Enter the title',
               }
           )
       )
       content = forms.CharField(
           widget=forms.TextInput(
               attrs={
                   'class': 'my_content form-control',
                   'placeholder': 'Enter the content'
               }
           ),
           error_messages={
               'required': 'Please enter your content!!!',
           }
       )
   ```

   ```html
   <!-- create.html -->
   
     <h2>2. Looping over the form's fields</h2>
     <form action="{% url 'articles:create' %}" method="POST">
       {% csrf_token %}
       {% for field in form %}
         {% if field.errors %}
           {% for error in field.erros %}
             <div class="alert alert-warning">{{ error }}</div>
           {% endfor %}
         {% endif %}
         {{ field.label_tag }}
         {{ field }}
       {% endfor %}
     </form>
     <input type="submit">
     <hr>
   ```

   

2. **Django Bootstrap 5 Library** 

   - 설치

     ```bash
     $ pip install django-bootstrap-v5
     ```

     ```bash
     $ pip install -e .
     ```

   - 등록

     ```python
     # settings.py
     
     INSTALLED_APPS = [
         'bootstrap5'
     ]
     ```

   - 단점: 자율성이 좀 떨어짐