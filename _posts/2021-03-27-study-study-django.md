---
layout: post
title:  "[Django] Django 설치&설정 및 내용 요약"
subtitle:   "수업시간에 배운 Django"
categories: study
tags: study Django
comments: true
---

> 수업 시간에 배운 Django 설치&설정 방법과 내용 정리



# Django

## 0. 장고 설치 및 기본 세팅

### bash

```bash
pip install django # django install

django-admin startproject firstpjt(프로젝트 이름)
django-admin startproject firstpjt . # 하위 폴더를 생성하지 않고 이 폴더에서 시작

python manage.py runserver # 서버가 잘 작동하는지 확인

python manage.py startapp articles(앱 이름) # app 먼저 생성 후 사용한다고 선언해주기
```



### settings.py

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'crud' / 'templates',], # 이 부분을 추가해준다
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# 일반적인 순서
#1. local apps
#2. 3rd-party apps
#3. django apps
INSTALLED_APPS = [
    'articles', # local (앱)
    'django_extensions', # 3rd-part apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# 언어 설정
LANGUAGE_CODE = 'ko-kr' # 한국어
TIME_ZONE = 'Asia/Seoul' # 서울 시간
```



### 처음 세팅

* urls.py / views.py / models.py

### firstpjt/urls.py

```python
from django.contrib import admin
from django.urls import path, include # include 추가로 import

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')), # articles.urls를 사용하겠다
]
```



### articles/urls.py

```python
from django.urls import path # 기본 프로젝트의 urls.py에서 path를 import
from . import views # articles의 views를 import

app_name='articles' # articles:index 처럼 사용할 때 필요
urlpatterns = [
    path('', views.index, name='index'),
    path('new/', views.new, name='new'),
    path('create/', views.create, name='create'),
    path('<int:pk>/detail/', views.detail, name='detail'),
    path('<int:pk>/edit/', views.edit, name='edit'),
    path('<int:pk>/update/', views.update, name='update'),
    path('<int:pk>/delete/', views.delete, name='delete'),
]

```



### views.py

```python
from django.shortcuts import render, redirect # redirect를 추가해준다
from .models import Article # models에서 Article이란 class를 import

# Create your views here.
def index(request):
    articles = Article.objects.all()[::-1]
	# articles = Article.objects.order_by('-pk')
    context = {
        'articles': articles # 이 정보를 바로 사용할 수 있다.
    }
    return render(request, 'index.html', context)

def new(request):
    return render(request, 'new.html')

def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')
    article = Article()
    article.title = title
    article.content = content
    article.save()
    context = {
        'article': article
    }
    return redirect('articles:index')

def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article
    }
    return render(request, 'detail.html', context)

def edit(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'edit.html', context)

def update(request, pk):
    article = Article.objects.get(pk=pk)
    title = request.POST.get('title')
    content = request.POST.get('content')
    article.title = title
    article.content = content
    article.save()
    context = {
        'article': article,
    }
    return redirect('articles:detail', article.pk)

def delete(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    else:
        return redirect('articles:detail', article.pk)
```



### base.html

```django
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
  <title>Document</title>
</head>
<body>
  {% raw %}
  {% block content %}
    <!-- 이 안에 내용이 들어가게 된다-->
  {% endblock content %} 
  {% endraw %}
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js" integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0" crossorigin="anonymous"></script>
</body>
</html>
```



### models.py

```python
from django.db import models

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)   

    # title을 돌려줌
    def __str__(self):
        return self.title
```



### admin.py

```python
from django.contrib import admin
from .models import Article

class ArticleAdmin(admin.ModelAdmin): # 나타낼 데이터를 바꿔줌
    list_display = ('pk', 'title', 'content', 'created_at', 'updated_at')

admin.site.register(Article, ArticleAdmin)
```



#### admin 계정 생성

```bash
$ python manage.py createsuperuserUsername (leave blank to use 'kihoon'): adminEmail address: Password: Password (again):The password is too similar to the username.This password is too short. It must contain at least 8 characters.This password is too common.Bypass password validation and create user anyway? [y/N]: ySuperuser created successfully.
```





### index.html

```django
{% raw %}
{% extends 'base.html' %} <!-- 반드시 extends해준다 -->{% block content %} <!-- 이후 block로 구역을 설정해준다 --><div style="margin:20px">  <h1>INDEX</h1>  <a href="{% url 'articles:new' %}">NEW</a>  <hr>  <br>  {% for article in articles %} <!--for문 사용-->    <h3>제목: {{ article.title }}</h3>    <h5>내용: {{ article.content }}</h5>    <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>    <hr>      {% endfor %}</div>{% endblock content %}
{% endraw %}
```



## 1. 각종 Tag

```python
{% raw %}
<h3>    1. for</h3>{% block content %}	{% for food in foods %}	<p>		{{ forloop.counter0 }}. {{ food }}    	</p>	{% endfor %}    {% for user in user_list %}        <p>{{ user }}</p>    {% empty %}        <p>지금 가입한 유저가 없습니다.</p>    {% endfor %}	<hr>    <h3>        2. if    </h3>    {% if '짜장면' in user_list %}        <p>짜장면엔 고춧가루 넣어야지!</p>    {% endif %}	{% for food in foods %}		{% if foorloop.first %}			<p>짜장면 + 고춧가루</p>		{% if forloop.last %}			<p>{{ food }} + 간장</p>		{% else %}			<p>{{ food }}</p>		{% endif %}	{% endfor %}	<hr>	<h3>    	3. length filter 활용    	</h3>	{% for fruit in fruits %}		{% if fruit|length > 5 %}			<p>이름이 너무 길어요</p>		{% else %}			<p>{{ fruit }}, {{ fruit|length }}</p>		{% endif %}	{% endfor %}	<hr>	<h3>        4. lorem ipsum	</h3>	{% lorem %}	<hr>	{% lorem 3 w %}	<hr>	{% lorem 4 w random %}	<hr>	{% lorem 2 p random %}	<hr>	{% lorem 1 b %}	<h3>    	5. 글자 관련 필터    	</h3>	<p>{{ 'ABC'|lower }}</p>	<p>{{ 'my_sentence'|title }}</p>	<p>{{ foods|random }}</p>	<h3>6. 연산</h3>	<p>{{ 4|add:6}}</p>	<hr>	<h3>7. 날짜 표현</h3>	{% now "DATETIME_FORMAT" %}	{% now "SHORT_DATETIME_FORMAT" %}	{% now "DATE_FORMAT" %}	{% now "SHORT_DATE_FORMAT" %}	<hr>	{% now "Y년 m월 d일 (D) A h:i" %}	{% now "Y" as current_year%}{% endblock %}
{% endraw %}
```



## 2. Migration

> Models.py 작성 후 적용 가능

> Object Relational Mapping(ORM)

* **makemigrations**
* **migrate**
  * 설계도를 실제 DB에 반영하는 과정
* sqlmigrate
* showmigrations



### shell_plus

* python manage.py shell_plus

```shell
In [1]: Article.objects.all()Out[1]: <QuerySet []>In [2]: ArticleOut[2]: articles.models.ArticleIn [3]: article = Article()In [4]: articleOut[4]: <Article: Article object (None)>In [5]: article.title = 'first'In [6]: article.titleOut[6]: 'first'In [7]: article.content = 'django!'In [8]: article.contentOut[8]: 'django!'In [11]: article.save()In [12]: articleOut[12]: <Article: Article object (1)>article = Article.objects.get(id=1)article = Article(title='second', content='django!')Article.objects.create(title='third', content='django!!')
```



### Read

* all()
* get()
  * 객체가 없으면 DoesNotExist 에러 발생
  * 객체가 여러개일 경우 MuiltipleObjectsReturned 에러 발생
  * 위와 같은 특징을 가지고 있기 때문에 unique 혹은 NOT NULL 특징을 가지고 있는 경우에만 사용 가능(`pk`로 조회)
* filter



### Field lookups(filter)

> get, filter, exclude에 대한 키워드 인수로 사용됨

```shell
In [2]: Article.objects.filter(content__contains='!')Out[2]: <QuerySet [<Article: first>, <Article: second>, <Article: third>]>In [3]: Article.objects.filter(pk__gt=1)Out[3]: <QuerySet [<Article: re_test>, <Article: second>, <Article: third>]>
```



### Delete

```python
# 선택되어있는 것을 삭제article.delete()article.delete()
```



## 3. Admin site

### Automatic admin interface

* 사용자가 아닌 서버의 관리자가 활용하기 위한 페이지
* Article class를 admin.py에 등록하고 관리
* django.contrib.auth 모듈에서 제공
* record 생성 여부 확인에 매우 유용하며 직접 record를 삽입할 수도 있음



## 4. POST

```django
{% raw %}
{% extends 'base.html' %}{% block content %}  <h1 style="margin-left:30px">NEW</h1>    <form action="{% url 'articles:create' %}", method="POST" style="margin-left:30px;">    {% csrf_token %} <!-- 이게 추가된다 -->    <label for="title">TITLE : </label>    <input type="text" placeholder="제목을 입력하세요" name="title" id="title">    <br><br>    <label for="content">CONTENT : </label>    <textarea name="content" id="content" cols="30" rows="10" placeholder="내용을 입력하세요"></textarea>    <br>    <input type="submit" value="작성">    <br>    <a href="{% url 'articles:index' %}">BACK</a>  </form>{% endblock content %}
{% endraw %}
```

* 이후 views에서 request.GET -> request.POST로 변경



## 5. redirect

```python
from django.shortcuts import render, redirectfrom .models import Articledef create(request):    pk = request.GET.get('pk')    title = request.GET.get('title')    content = request.GET.get('content')    article = Article()    article.title = title    article.content = content    article.save()    pk = article.pk    created_at = article.created_at    context = {        'pk': pk,        'title': title,        'content': content,        'created_at': created_at,    }    # return render(request, 'create.html', context)    return redirect('articles:detail', pk) # detail 함수를 다시 실행
```



## 6. 가상 환경(venv)

> 프로젝트를 생성할 때, 그 프로젝트에 필요한 라이브러리나 파일만 설치하므로 확실히 나타낼 수 있다.

```bash
# 가상환경(venv) 생성python -m venv venv# 가상환경(venv) 실행source venv/Scripts/activate# 아무것도 없는 환경이기 때문에 다시 설치해준다.pip install django# 가상환경(venv) 실행 중지deactivate
```

* Django를 `gitignore`에 등록하면 `sqlite3`도 포함된다.

 

* README.md 에는 python 버전을 명시하는게 좋다.
  * [![Generic badge](md-images/python-3.8.7-blue)](https://shields.io/)

### 그 외 알아야 할 명령어

```bash
# pip 설치 리스트를 가져온다. 바로 밑 명령어는 사용해보지 않았다.pip -m pip freeze (options)# 프로젝트 때는 이 명령어를 사용, txt 파일이 생성되며 pip list를 저장한다.pip freeze > requirements.txt# requirements.txt를 사용하는 방법, -r이 핵심인 듯pip install -r requirements.txt# movies.json 가져와 파일로 저장해주는 방법# "model": "movies.movie" => 앱.클래스 형태python manage.py dumpdata movies.movie# 현재 데이터 옵션 주며 저장하기python manage.py dumpdata --indent 4 movies.movie > movies.json# 가져와 사용하기, movies/movies.json은 터미널에서의 상대적 위치python manage.py loaddata movies/movies.json 
```



## 7. Form

* forms.py

```python
from django import formsclass ArticleForm(forms.Form):    REGIONS = [        ('sl', '서울'),        ('dj', '대전'),        ('gj', '광주'),        ('gm', '구미'),    ]        title = forms.CharField(max_length=10)    content = forms.CharField(widget=forms.Textarea)    region = forms.ChoiceField(choices=REGIONS, widget=forms.RadioSelect())    
```

* views.py

```python
def new(request):    form = ArticleForm()    context = {        'form': form,    }    return render(request, 'articles/new.html', context)
```

* new.html

```django
{% raw %}
{% extends 'base.html' %}{% block content %}  <h1 class="text-center">New Article</h1>  <hr>  <form action="#" method="POST">    {% csrf_token %}    {{ form.as_p }}  <!-- p 속성 -->    {{ form.as_ul }} <!-- ul 속성 -->    {{ form.as_table }} <!-- table 속성 -->    <input type="submit" value="글 작성">  </form>{% endblock content %}
{% endraw %}
```



### ModelForm

* forms.py

```python
from django import formsfrom .model import Articleclass ArticleForm(forms.ModelForm):    class Meta:        models = Article        fields = '__all__' # model에서 정의한 속성을 전부 다 보여주겠다#        exclude = ('updated_at',) # 이건 제외하겠다#        fields = ['pk', 'title', 'content'] # 이것만 보여주겠다
```



* views.py -> new 함수를 create 함수에 합침

```python
def create(request):    if request.method == 'POST':        form = ArticleForm(request)        if form.is_vaild():            article = form.save()            return redirect('articles:index', article.pk)        return redirect('article:index')    else:        form = ArticleForm()        context = {            'form': form,        }        return render(request, 'articles/new.html', context)    # 데이터 가져오려면 instance 사용def update(request, pk):    movie = Movie.objects.get(pk=pk)    if request.method == 'POST':        form = MovieForm(request.POST, request.FILES, instance=movie)        if form.is_valid():            movie = form.save()            return redirect('movies:detail',movie.pk)    else:        form = MovieForm(instance=movie)    context = {        'form':form,        'movie':movie,    }    return render(request, 'movies/form.html', context)
```

* new.html

```django
{% raw %}
{% extends 'base.html' %}{% block content %}  <h1 class="text-center">New Article</h1>  <hr>  <form action="" method="POST"> <!-- action을 비워두면 자기 자신에게 다시 요청함 -->    {% csrf_token %}      <!-- 셋 중 하나 사용 -->    {{ form.as_p }}    {{ form.as_ul }}    {{ form.as_table }}    {% comment %}     	제목: <input type="text" name="title"><br>    	내용: <textarea name="content" id="" cols="30" rows="10"></textarea><br>     {% endcomment %}    <input type="submit" value="글 작성">  </form>{% endblock content %}
{% endraw %}
```



* forms.py 에서 ModelForm 사용

```python
class ArticleForm(forms.ModelForm):    title = forms.CharField(        label = '제목',        widget = forms.TextInput(            attrs = {                'class': 'title',                'placeholder': 'Enter the title',            }        )	    )    content = forms.CharField(        label = '내용',        widget = forms.Textarea(            attrs = {                'class': 'content',                'placeholder': 'Enter the content',                'rows': 5,                'cols': 30,            }        ),        error_messages={            'required': '데이터를 입력해줄래?', # 에러 메시지 바꾸려면        }    )    # bootstrap5 사용시 class에 form-control    poster_path = forms.CharField(        label="영화 포스터 링크",        required=False,        widget=forms.TextInput(            attrs={                'class':'form-control',                # 'class':'poster_path',                'placeholder':'URL을 입력해주세요',            }        )    )    class Meta:        model = Article        fields = '__all__' # model에서 정의한 속성을 전부 다 보여주겠다        # fields = ['pk', 'title', 'content']
```



### HTML에서 사용

```django
{% raw %}
<!-- 기본 -->
<form action="" method="POST">
    {% csrf_token %}
    <div>
        {{ form.title.errors }}
        {{ form.title.label_tag }}
        {{ form.title }}
    </div>
</form>

<!-- for문 -->
<form action="" method="POST">
    {% csrf_token %}
    {% for field in form %}
    	{{ field.errors }}
    	{{ field.label_tag }}
    	{{ field }}
    {% endfor %}
    <input type="submit">
</form>

<!-- 응용 -->
<form action="" method="POST">
    {% csrf_token %}
    {% for field in form %}
    	{% if field.errors %}
    		<ol>
            {% for error in field.errors %}
                <li class="alert alert-warning" role="alert"><strong>{{ error|escape }}</strong></li> <!-- 이렇게 class를 이용해 alert 속성을 바꿔줄 수도 있음 -->
            {% endfor %}
    		</ol>
    	{% endif %}
    	{{ field.errors }}
    	{{ field.label_tag }}
    	{{ field }}
    {% endfor %}
    <input type="submit">
</form>
{% endraw %}
```



## 8. django-bootstrap-v5

### 기본 설정

```bash
# install
pip install django-bootstrap-v5
# 이후에 venv에서 requirements.txt를 최신화
pip freeze > requirements.txt

# settings.py에서 installed_app에 추가
INSTALLED_APPS = [
	'...',
	'bootstrap5',
	'...',
]
```



### 사용

* base.html에서 사용

```django
{% raw %}
{% load bootstrap5 %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  {% bootstrap_css %} <!-- CDN 대신 이렇게 사용 가능 -->
  <title>Document</title>
</head>
<body>
  <div class="container">
    {% block content %}
    {% endblock content %}
  </div>
  {% bootstrap_javascript %}
</body>
</html>
{% endraw %}
```



* html에서 사용

```django
{% raw %}
{% extends 'base.html' %}{% load bootstrap5 %}{% block content %}<form action="" method="POST" class="d-inline">  	{% comment %} {% bootstrap_form form %} {% endcomment %} <!-- form을 불러온다 --> 	{% bootstrap_form form layout='horizontal' %} <!-- 속성이 추가된 form -->	{% buttons %}		<button type="submit" class="btn btn-primary">Submit</button>	{% endbuttons %}	{% buttons submit='OK' reset="Cancel" %}{% endbuttons %}</form>{% endblock content %}
{% endraw %}
```



## 9. include html

* pjt/templates/nav.html 에 nav를 넣고

* base.html

```django
{% raw %}
{% load bootstrap5 %}<!DOCTYPE html><html lang="en"><head>  <meta charset="UTF-8">  <meta http-equiv="X-UA-Compatible" content="IE=edge">  <meta name="viewport" content="width=device-width, initial-scale=1.0">  {% bootstrap_css %}  <title>Document</title></head><body>  {% include 'nav.html' %} <!-- 이렇게도 사용 가능 -->  <div class="container">    {% block content %}    {% endblock content %}  </div>  {% bootstrap_javascript %}</body></html>
{% endraw %}
```



## 10. View decorators

>어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능을 연장해주는 함수

### Allowed HTTP methods

> 요청 메서드에 따라 view 함수에 대한 엑세스를 제한  



* views.py

```python
from django.shortcuts import render, redirectfrom .models import Articlefrom .forms import ArticleFormfrom django.views.decorators.http import require_safe, require_http_methods, require_POST# index는 GET 방식을 사용하는게 낫다. GET 대신 safe를 사용하는걸 권장# 여기서 POST나 PUT을 사용하면 'method not allowed' 에러 발생@require_safedef index(request):    articles = Article.objects.all()    context = {        'articles': articles,    }    return render(request, 'articles/index.html', context)# GET과 POST를 사용하고 싶다@require_http_methods(['GET', 'POST'])def create(request):    # POST일 때    if request.method == 'POST':        form = ArticleForm(request.POST)        if form.is_valid():            article = form.save()            return redirect('articles:detail', article.pk)    # GET일 때    else:        form = ArticleForm()    context = {        'form': form,    }    return render(request, 'articles/create.html', context)@require_http_methods(['GET', 'POST'])def update(request, pk):    article = Article.objects.get(pk=pk)    if request.method == 'POST':        form = ArticleForm(request.POST, instance=article)        if form.is_valid():            article = form.save()            return redirect('articles:detail', article.pk)    else:        form = ArticleForm(instance=article)    context = {        'form': form,        'article': article,    }    return render(request, 'articles/update.html', context)@require_safedef detail(request, pk):    article = Article.objects.get(pk=pk)    context = {        'article': article    }    return render(request, 'articles/detail.html', context)# 만약 GET으로 처리하려 한다면 '405 error' 발생@require_POSTdef delete(request, pk):    article = Article.objects.get(pk=pk)    article.delete()    return redirect('articles:index')
```



### 그 외 Tips

* POSTMAN 을 사용하여 PUT, DELETE 등의 요청도 보내볼 수 있음

* settings.py에서 MIDDLE_WARE의 `csrf`를 주석처리하면 토큰 검사를 안함



## 11. static

> 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 응답하면 되는 파일

* 기본 static 경로
  * app_name/static/
* 사용하기

```django
{% raw %}
{% load static %}<img src="{% static 'articles/sample.png' %} alt="sample">
{% endraw %}
```

> articles(app_name) / static / articles / sample.png 라는 파일을 불러오는 것



* **STATIC_ROOT**
  * 배포 단계에서 collectstatic이 배포를 위해 정적 파일을 수집하는 절대 경로
  * 개발 단계에서는 STATIC_ROOT 경로를 작성하지 않아도 문제없이 동작



* **STATIC_URL**
  * 실제 파일이나 디렉토리가 아니며, URL로만 존재
  * 비어 있지 않은 값으로 설정한다면 반드시 `/`로 끝나야 함
  * `/static/`



* articles/static/articles/ 파일 생성
  * 127.0.0.1:8080/static/articles/sample.png 형태이기 때문에 다른 앱에서 이름이 겹치는 경우를 대비하기 위해서 articles라는 app name과 같은 디렉토리를 하나 추가해주는 것이다.



* **STATICFILES_DIRS** = [

  	BASE_DIR / 'crud' / 'static',

  ]

  * app 내의 static 디렉토리 경로를 사용하는 것 외에 추가 경로를 지정



* base.html 에서 block css도 가능하다!
* 그러고 아래 index

```django
{% raw %}
{% load static %}{% block css %}	<link rel="stylesheet" href="{% url 'stylesheets/style.css' %}">{% endblock %}<!-- crud에서 static/stylesheets/style.css 생성 -->
{% endraw %}
```



* 127.0.0.1:8080/static/app_name/image_name.jpg 로 작동되기 때문에 crud 하위에 있든 articles같이 앱 하위에 있든 둘 다 찾게 된다.



## 12. media

> image, pdf, video 등

* **MEDIA_ROOT**
  * 사용자가 업로드 한 파일을 보관할 디렉토리의 절대 경로
  * 실제 해당 파일의 업로드가 끝나면 파일이 저장되는 경로
  * Django는 성능을 위해 업로드 파일은 데이터베이스에 저장하지 않음
    * DB에 저장되는 것은 파일의 경로



* **MEDIA_URL**
  * 비어 있지 않는 값으로 설정한다면 반드시 `/`로 끝나야 함
  * MEDIA_ROOT와 STATIC_ROOT는 서로 다른 경로를 가져야 함



* 파일을 저장하기 위해선 models.py를 수정해야 한다(db에 넣기 위함)

```python
from django.db import modelsclass Article(models.Model):    title = models.CharField(max_length=10)    content = models.TextField()    image = models.ImageField(blank=True) # null 허용    created_at = models.DateTimeField(auto_now_add=True)    updated_at = models.DateTimeField(auto_now=True)    def __str__(self):        return 'No.{} - {}'.format(self.pk, self.title)
```

* ImageField 사용을 위해선 `Pillow`를 install

```bash
pip install Pillowpython manage.py makemigrationspython manage.py migrate
```



* create.html 파일

```django
{% raw %}
{% extends 'base.html' %}{% block content %}  <h1 class="text-center">New Article</h1>  <hr>  <form action="" method="POST" enctype="multipart/form-data"> <!-- enctype 추가 -->    {% csrf_token %}    {{ form.as_p }}    {% comment %} {{ form.as_ul }}    {{ form.as_table }} {% endcomment %}    {% comment %} 제목: <input type="text" name="title"><br>    내용: <textarea name="content" id="" cols="30" rows="10"></textarea><br> {% endcomment %}    <input type="submit" value="글 작성">  </form>  <a href="{% url 'articles:index' %}">BACK</a>{% endblock content %}
{% endraw %}
```

* views.py

```python
def create(request):    if request.method == 'POST':		form = ArticleForm(request.POST, requests.FILES) # FILES도 request해야 한다        if form.is_valid():            article = form.save()            return redirect('articles:detail', article.pk)    else:        form = ArticleForm()    context = {        'form': form,    }    return render(request, 'articles/create.html', context)
```



* detail.html

```django
{% raw %}
{% extends 'base.html' %}

{% load static %}

{% block content %}
  
  {% comment %} https://getbootstrap.com/docs/5.0/components/card/#image-caps {% endcomment %}
  <div class=" mb-3 offset-md-2 col-md-8 col-12">
    <div class="card">
      {% if movie.poster_file %}
        {% comment %} 파일 존재O {% endcomment %}
        <img src="{{ movie.poster_file.url }}" class="card-img-top" alt="...">
      {% else %}
        {% comment %} 파일 존재X => 이미지 URL {% endcomment %}
        <img src="{{ movie.poster_path }}" class="card-img-top" alt="..." onerror="this.src='{% static 'movies/placeholder_movie.jpg' %}'">
      {% comment %} {% else %}
        <img src="{% static 'movies/placeholder_movie.jpg' %}" class="card-img-top" alt="..."> {% endcomment %}
      {% endif %}
      <div class="card-body">
        <h5 class="card-title">{{ movie.title }}</h5>
        <p class="card-text">{{ movie.overview }}</p>
        
      </div>
    </div>
    <div class="d-flex justify-content-end mt-2">
      <a href="{% url 'movies:update' movie.pk %}" class="btn btn-primary">UPDATE</a>
      <form action="{% url 'movies:delete' movie.pk %}" method="post" class="d-inline">
        {% csrf_token %}
        <input type="submit" class="btn btn-danger mx-2" value="DELETE">
      </form>
      <a href="{% url 'movies:index' %}" class="btn btn-secondary">BACK</a>
    </div>
  </div>
{% endblock content %}
{% endraw %}
```



* forms.py

```python
from django import forms
from .models import Movie

class MovieForm(forms.ModelForm):
    title = forms.CharField(
        label="영화 제목",
        widget=forms.TextInput(
            attrs={
                # 'class':'title',
                'class':'form-control',
                'placeholder':'제목을 입력해주세요',

            }
        )
    )
    
    overview = forms.CharField(
        label="영화 개요",
        widget=forms.Textarea(
            attrs={
                'class':'form-control',
                # 'class':'overview',
                'placeholder':'내용을 입력해주세요',
            }
        )
    )

    poster_path = forms.CharField(
        label="영화 포스터 링크",
        required=False,
        widget=forms.TextInput(
            attrs={
                'class':'form-control',
                # 'class':'poster_path',
                'placeholder':'URL을 입력해주세요',
            }
        )
    )

    # poster_file = forms.ImageField(
    #     label="영화 포스터 파일",
    #     # <br>을 제거하기 위한 여정
    #     # widget=forms.FileInput(
    #     #     attrs={
    #     #         'class':'d-flex align-content-start',
    #     #     }
    #     # )
    # )

    class Meta:
        model = Movie
        fields = '__all__'
        def __init__(self, *args, **kwargs):
            super(MovieForm, self).__init__(*args, **kwargs)
            self.fields['file'].required=False
```





* settings.py

```python
# 가장 아래에 추가MEDIA_ROOT = BASE_DIR / 'media'MEDIA_URL = '/media/'
```



* urls.py

```python
from django.contrib import adminfrom django.urls import path, includefrom django.conf import settings			# setting를 importfrom django.conf.urls.static import static	# static를 importurlpatterns = [    path('admin/', admin.site.urls),    path('articles/', include('articles.urls')),] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # static 추가
```



* update의 html과 views도 수정한다.



* admin.py

```python
from django.contrib import adminfrom .models import Articleclass ArticleAdmin(admin.ModelAdmin):    list_display = ['pk', 'title', 'image', 'created_at', 'updated_at']admin.site.register(Article, ArticleAdmin)
```



* index.html에서 보여주는 방법
* src에 넣을 url 경로를 찾는다면 **movie.poster_file.url**

```django
{% raw %}
{% extends 'base.html' %}{% load static %}{% block content %}  {% comment %} data-masonry='{"percentPosition": true }' {% endcomment %}  <div class="row row-cols-1 row-cols-md-3 g-4" data-masonry='{"percentPosition": true }'>    {% for movie in movies %}      <a href="{% url 'movies:detail' movie.pk %}" class="text-decoration-none text-dark">        <div class="col">          <div class="card">            {% comment %} <img src="{{ movie.poster_path }}" class="card-img-top" alt="movie poster" onerror="this.src='{% static 'movies/placeholder_movie.jpg' %}'"> {% endcomment %}            {% if movie.poster_file %}              {% comment %} 파일 존재O {% endcomment %}              <img src="{{ movie.poster_file.url }}" class="card-img-top" alt="...">            {% else %}              {% comment %} 파일 존재X => 이미지 URL {% endcomment %}              <img src="{{ movie.poster_path }}" class="card-img-top" alt="..." onerror="this.src='{% static 'movies/placeholder_movie.jpg' %}'">            {% endif %}            <div class="card-body">              <h5 class="card-title">{{ movie.title }}</h5>              <p class="card-text">{{ movie.overview }}</p>            </div>          </div>        </div>      </a>    {% endfor %}  </div>{% endblock content %}
{% endraw %}
```





### form을 사용하지 않고 이미지 업로드

* create.html

```django
<form>제목: <input type="text" name="title"><br>내용: <textarea name="content" id="" cols="30" rows="10"></textarea><br><input type="file" name="image" accept="image/*"><input type="submit"></form>
```



* 다운받은 파일 실행

```bash
# 가상환경 생성python -m venv venv# 가상환경 실행source venv/Scripts/activate# 필요 파일 다운로드pip install -r requirements.txt# dbpython manage.py migrate
```



## 13. django-imagekit

image = models.ImageField(blank=True)

* bash

```bash
pip install django-imagekit
```

* settings.py

```python
INSTALLED_APP = [    ...,    imagekit,    ...,]
```



### 이미지를 Cache로 저장

> thumbnail이 사용될 때 그때그때 resize

* models.py

```python
from django.db import models 
from imagekit.models import ImageSpecField
from imagekit.processors import ResizeToFill

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextFIeld()
    image = models.ImageField(blank=True)
    image_thumbnail = ImageSpecField(source='image',
                                    processors=[ResizeToFill(200, 200)],
                                    format='JPEG',
                                     options={'quality':60})
    created_at = models.DateTimeField(auto_now_add=True)
    
post = Post.objects.all()[0]
{% raw %}
# 이걸 {{ }} 안에 넣어서 불러올 수 있다.
{% endraw %}
post.image_thumbnail.url
```



### 이미지 크기 자체를 조절

> 이미지를 처리해놓고 사용하기 때문에 DB를 미리 makemigrations 

* models.py

```python
from django.db import models 
from imagekit.models import ProcessedImageField
from imagekit.processors import ResizeToFill

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextFIeld()
    # image = models.ImageField(blank=True)
    # upload to는 어떤 폴더에 넣을지
    image = ProcessedImageField(upload_to='images/%Y/%m/%d/', 
                                    processors=[ResizeToFill(500, 500)],
                                    format='JPEG',
                                     options={'quality':100})
    created_at = models.DateTimeField(auto_now_add=True)
    
# post = Post.objects.all()[0]
{% raw %}
# 밑줄에 이걸 {{}} 안에 넣어서 불러올 수 있다.
{% endraw %}
post.image.url
```





## 14. form.html

> create와 update는 유사한 면이 많아 하나로 통합하여 사용한다.
>
> 유지/보수를 위해 나중에 form.html을 include하는 방법도 좋을 것 같다.

* form.html

```django
{% raw %}
{% extends 'base.html' %}{% load bootstrap5 %}{% block content %}  {% if request.resolver_match.url_name == 'create' %}    <h2>CREATE</h2>  {% else %}    <h2>UPDATE</h2>  {% endif %}  <form action="" method="POST" class="d-inline" enctype="multipart/form-data">    {% csrf_token %}    {% comment %} {{ form.as_p }} {% endcomment %}    {% bootstrap_form form layout="horizontal" %}    <div class="d-flex justify-content-end mt-2">      {% comment %} {% buttons submit="제출" %} <a href="{% url 'movies:index' %}" class="btn btn-secondary">BACK</a> {% endbuttons %} {% endcomment %}      {% bootstrap_button "SAVE" button_type="submit" button_class="btn-primary mx-2" %}      {% bootstrap_button "BACK" button_type="link" href='/movies' button_class="btn-secondary" %} <!-- 버튼의 사용법 -->    </div>        {% comment %} <input type="submit" value="제출" class="btn btn-primary"> {% endcomment %}  </form>  {% comment %} <a href="{% url 'movies:index' %}" class="btn btn-secondary">BACK</a> {% endcomment %}{% endblock content %}
{% endraw %}
```





## 15. 500 에러 방지

* views.py

```python
from django.shortcuts import render, redirect, get_object_or_404def detail(request, pk):#    post = Post.objects.get(pk=pk)	post = get_object_or_404(Post, pk=pk)    context = {        'post': post,    }    return render(request, 'posts/detail.html', context)
```



### 404 Page remodeling

> **settings.py**에서 **DEBUG = FALSE**로 바꾸고 404.html을 template tree의 최상단에 넣는다.

> awesome 404 참고





## 16. git undo

```bash
# a.txt와 b.txt가 있음git init# a.txt 수정 후# b.txt는 working directory에 있고 a.txt는 staging area로 올라갔다.git add a.txt# a.txt를 working directory로 내리고 싶다면git rm --cached a.txt# a.txt를 수정 후 commit까지 해보자git add a.txtgit commit -m 'commit a.txt'# a.txt를 다시 수정 후 addgit add a.txt# 깃의 상태 확인git status# 한번 업로드 된 상태에서 수정 전 단계로 이동한다git restore --staged a.txt# commit에서 오타가 남git add .git commit -m 'gitaddasd'# 커밋을 취소git commit --amend# vim 화면으로 넘어가서# i를 눌러 '--끼워넣기--' 로 커밋 메시지 수정esc 후 :wq# c.txt와 d.txt 생성# 실수로 d를 빼먹고 commit함git add c.txtgit commit -m 'update c,d'# d.txt를 commit에 추가하고 싶다git add d.txt# c를 내린다git commit --ammend# vim으로 이동되면서 c,d가 staging area에 올라가있는게 보인다.# 저장 후 나가면 commit된다:wq
```



### 여러 커밋으로 옮겨다니기

> 폴더에서 git init을 하고 README.md 파일 생성 후 작업

* README.md

```markdown
# 영화 보기1. 영화를 보기 위해 영화관 도착
```

* bash

```
git add .git commit -m "영화관 도착"
```



* README.md

```markdown
# 영화 보기

1. 영화를 보기 위해 영화관 도착
2. 명량 영화표 구매
```

* bash

```
git add .
git commit -m "영화표 구매"
```



* README.md

```markdown
# 영화 보기

1. 영화를 보기 위해 영화관 도착
2. 명량 영화표 구매
3. 팝콘 구매
```

* bash

```
git add .
git commit -m "팝콘 구매"
```



* README.md

```markdown
# 영화 보기

1. 영화를 보기 위해 영화관 도착
2. 명량 영화표 구매
3. 팝콘 구매
4. 상영시간이 될때까지 웹서핑
```

* bash

```
git add .git commit -m "웹서핑"
```



* README.md

```markdown
# 영화 보기

1. 영화를 보기 위해 영화관 도착
2. 명량 영화표 구매
3. 팝콘 구매
4. 상영시간이 될때까지 웹서핑
5. 댓글 읽다가 주인공 사망 스포당함
```

* bash

```
git add .
git commit -m "스포당함"
```



* README.md

```markdown
# 영화 보기

1. 영화를 보기 위해 영화관 도착
2. 명량 영화표 구매
3. 팝콘 구매
4. 상영시간이 될때까지 웹서핑
5. 댓글 읽다가 주인공 사망 스포당함
6. 나도 스포해야지 댓글 작성
```

* bash

```
git add .
git commit -m "댓글작성"
```



* README.md

```markdown
# 영화 보기

1. 영화를 보기 위해 영화관 도착
2. 명량 영화표 구매
3. 팝콘 구매
4. 상영시간이 될때까지 웹서핑
5. 댓글 읽다가 주인공 사망 스포당함
6. 나도 스포해야지 댓글 작성
7. 영화를 재미없게 보고 나옴
```

* bash

```
git add .
git commit -m "영화 보고 나옴"
```



```bash
# 로그를 찍어본다
$ git log --online
6d0784b 영화표 구매
8e8d5e4 영화관 도착
```



> 위 과정 진행 후 3가지 방식으로 해보면...

#### hard

```bash
git reset --hard 86c4d61
```

* 그때로 코드를 비롯해 모든 옵션을 아예 돌려버림



#### soft

```bash
git reset --soft 86c4d61
# 상태를 찍어보면 돌아가 전의 코드 그대로가 staging area로 올라가 있다.
git status   
```

* 코드는 바뀌지 않고 commit 만 바뀜



#### mixed

```bash
# mixed는 default
$ git reset 86c4d61
# 상태를 찍어보면 돌아가 전의 코드가 working directory로 올라가 있다.
```
