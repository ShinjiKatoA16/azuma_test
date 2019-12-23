# Django installation

## Install miniconda

- execute Miniconda-Latest-Windows_x86_64 in Q:\04_Python
- Confirm python interactive shell
  + open command terminal START->Anaconda3->Anaconda Prompt
  + `python`
  + type `exit()` to terminate python shell
- Edit python program using ATOM or notepad (ATOM is preferred)

## Virtual environment Mangement excersize
Following conda commands are used. Change env_name to proper one.

- `conda info -e`
- `conda create -n env_name python=xx.xx`
- `conda activate env_name`
- `conda deactivate`
- `conda remove -n env_name` --all

### Sample operation

#### Crate new virtual environment py35

- `conda info -e`
- `conda create -n py35 python=3.5`
- `conda info -e`

#### install package on py35

- `conda activate py35`
- Confirm that envrionment name in prompt is changed to **(py35)**
- `pip list`
- `pip install numpy`
- `pip list`

#### Return to base envrironment and check that numpy is not installed

- `conda deactivate`
- `pip list`

#### Remove py35

- `conda remove -n py35` --all
- `conda info -e`

## Create virtual envrionment for Django

- `conda create -n django python~=3.7`
- `conda activate django`
- `pip install Django`
- `pip list`

# Django Application

## Create working directory for Django project

- move to your home directory (C:\Users\NExxxxx)
- `mkdir djangogirls`
- `cd djangogirls`
- Confirm that directory is changed properly (C:\Users\NExxxxx\djangogirls)

## Create Django project

- Confirm that virtual envrinment **django** is activated. Prompt is (django)C:\Users\NExxxx\djangogirls
- `django-admin.exe startproject mysite .`  <- take care not to forget period.
- `dir`
- <- confirm that manage.py and mysite exist
- run web server
- `python manage.py runserver`
- Access the server 127.0.0.1:8000 using web browser (Chrome)
- Check the source code of HomePage (which is dynamically created)
- Terminate the web server Ctrl-C (Ctrl-Break  Ctrl-Fn-B ?)

### Change settings

- `atom mysite\settings.py`
  + TIME_ZONE = 'Asia/Tokyo'
  + LANGUAGE_CODE = 'ja'
  + STATIC_ROOT = os.path.join(BASE_DIR, 'static')
  
Try other than **ja** in LANGUAGE_CODE. 
STATIC_ROOT is to be written after STATIC_URL = '/static/'

### Migrate

- `python manage.py migrate`

### Verification

Restart web server

- `python manage.py runserver`
- Access the server 127.0.0.1:8000 using web browser (Chrome)
- Terminate the web server Ctrl-C (Ctrl-Break  Ctrl-Fn-B ?)

## Blog post

We will create **Blog Post** application in this tutorial.

- `python manage.py startapp blog`
- `dir`  <- confirm that **blog** sub-directory is created under djangogirls
- Modify mysite\settings.py, (add blog.apps.BlogConfig to INSTALLED_APPS section) 

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
]
```

- `atom blog\models.py`

```
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

- `python manage.py makemigrations blog`
- `python manage.py migrate blog`

### Django admin

- `atom blog\admin.py`

```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

- `python manage.py createsuperuser`
- `python manage.py runserver`
- access 127.0.0.1:8000/admin
- add several blog

# Deploy

Read https://tutorial.djangogirls.org/ja/deploy/

- Create GitHub account
   + https://github.com/  User ID を作成するために sign up をクリック
   + Install ATOM and Git, Q:\04_python\01_開発環境
   + set environment variable https://did2memo.net/2017/03/15/windows-set-environment-variables-without-admin-auth/
   + https://techacademy.jp/magazine/6235
   + git を install したら最初に一度だけ global に名前と e-mail address を登録。この情報が Commit に記録される
      - `git config --global user.name "Your Name"`
      - `git config --global user.email you@example.com`
- ローカル側で repository を作成し、Github と関連づける
   + github で my-first-blog という名前でカラの repository をつくる
   + `cd C:\users\you_user_id\djangogirls`
   + `git init`
   + `git remote add origin https://github.com/<your-github-username>/my-first-blog.git`
- .gitignore というファイルを作成する。これらのファイルは add, commit の対象にならない
```
*.pyc
*~
__pycache__
myvenv
db.sqlite3
/static
.DS_Store
```
- へんこうを github の repository に push する
   + `git add --all .`
   + `git commit -m "My Django Girls app, first commit"`
   + `git push -u origin master`
- Create PythonAnywhere account
  + https://www.pythonanywhere.com にアクセスして User ID を作成する。無料の Beginner をえらぶ。User 名は URL のいちぶになるので良い名前をえらぶ
  + pythonanywhere でトークンをさくせい。Account -> API Token -> Create a new API Token
  + pythonanywhere の画面で bash をうごかして以下のコマンドをにゅうりょく。（プロジェクトごとに1回だけ）
     - pip3.6 install --user pythonanywhere
     - pa_autoconfigure_django.py https://github.com/your-github-username/my-first-blog.git (your-github-username を自分の ID におきかえる)
     - python manage.py createsuperuser
  + ユーザID.pythonanywhere.com にアクセスしてうごいていることをかくにん
  + pythonanywhere から抜けたあとに、再度 bash を立ち上げた場合は `workon ユーザーID.pythonanywhere.com` を実行して仮想環境を activate して `cd ユーザーID.pythonanywhere.com` でディレクトリを移動する
  + いちど作成した web application を削除する場合は Login した後の画面で **Web** のタブをクリック -> 画面の一番下で `Delete xxxx.pythonanywhere.com` のボタンをクリック。再度 pa_autoconfigure_django.py を --nuke オプションを付けて実行する
     
# よく使う Git command

ほかにも色々便利な機能がある。"git 使い方" をキーワードにして検索すればたくさん記事がある。[Pro git](https://progit-ja.github.io/) がよくまとまっている。

## Github から Repository を copy

- git clone https://github.com/user_name/repository_name

## local repository のじょうたいをかくにん

- git status

## ファイルの追加・変更

- git add ファイル名
- git commit ファイル名 -m "コメント"

## commit を Remote repository に反映

- git push

## commit のりれきを確認

- git log

# DjangoGirls トップページにスタティックな HTML　を表示

## Django  urls, view, template

  - https://www.sejuku.net/blog/26407
  - https://djangobrothers.com/tutorials/memo_app/mtv/
  - https://tutorial.djangogirls.org/ja/django_urls/
  - https://tutorial.djangogirls.org/ja/django_views/
  - https://tutorial.djangogirls.org/ja/html/
  
## what to do

  - `atom mysite\urls.py`
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```
  - `atom blog\urls.py`
  
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```

  - `atom blog\views.py`
  
```
from django.shortcuts import render

def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

  - `atom blog\templates\blog\post_list.html`
  
```
<html>
    <head>
        <title>Django Girls blog</title>
    </head>
    <body>
        <div>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div>
            <p>published: 14.06.2014, 12:14</p>
            <h2><a href="">My first post</a></h2>
            <p>Aenean eu leo quam. こんにちは！ よろしくお願いします！ </p>
        </div>

        <div>
            <p>公開日: 2014/06/14, 12:14</p>
            <h2><a href="">2番目の投稿</a></h2>
            <p> こんにちは！ よろしくお願いします！ </p>
        </div>
    </body>
</html>
```

# ふたたび Deploy

- Local のへんこうを github に upload
  + djangogirls の directory に cd コマンドで移る  `cd djangogirls`
  + `git status`
  + `git add --all .`
  + `git status`
  + `git commit -m "Change the HTML for the site" `
  + `git push`
- github の変更を pythonanywhere にはんえいさせる
  + bash をひらく
  + 自分の User-ID.pythonanywhere.com の directory にうつる  `cd User-ID.pythonanywhere.com`
  + `git pull`
- pythonanywhere の web の tab をひらいて、**Reload** のボタンをクリック
- User-ID.pythonanywhere.com を web browser でひらく

# ORM hands on

https://tutorial.djangogirls.org/ja/django_orm/

# Template 内の動的データ

- `atom blog\views.py`

```
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```

- `atom blog\templates\blog\post_list.html`

```
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h2><a href="">{{ post.title }}</a></h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

## django の組み込みタグのリファレンス

- [Django Document 日本語](https://djangoproject.jp/doc/ja/1.0/ref/templates/builtins.html)
- [Django Document 英語](https://docs.djangoproject.com/en/3.0/ref/templates/builtins/)

# CSS (Cascading Style Sheet)

## install bootstrap(CSS framework)

- `atom blog\templates\blog\post_list.html`

```
<head>
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
</head>

<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h2><a href="">{{ post.title }}</a></h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

- `python manage.py runserver`
- access 127.0.0.1:8000 using web browser (color of header is now blue)

## Create CSS file

- `atom blog\static\css\blog.css`

```
h1 a, h2 a {
    color: #C25100;
}
```

- `atom blog\templates\blog\post_list.html`

```
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        {% for post in posts %}
            <div>
                <p>published: {{ post.published_date }}</p>
                <h2><a href="">{{ post.title }}</a></h2>
                <p>{{ post.text|linebreaksbr }}</p>
            </div>
        {% endfor %}
    </body>
</html>
```

- reload 127.0.0.1:8000 (color of header is now orange)

### try other color

- Google で CSS color を検索

## Adjust left padding

- `atom blog\static\css\blog.css`

```
h1 a, h2 a {
    color: #C25100;
}

body {
    padding-left: 15px;
}
```

- reload 127.0.0.1:8000 (left padding is inserted)

## Change font

- `atom blog\templates\blog\post_list.html`

```
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href="//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext" rel="stylesheet" type="text/css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        {% for post in posts %}
            <div>
                <p>published: {{ post.published_date }}</p>
                <h2><a href="">{{ post.title }}</a></h2>
                <p>{{ post.text|linebreaksbr }}</p>
            </div>
        {% endfor %}
    </body>
</html>
```

- `atom blog\static\css\blog.css`

```
h1 a, h2 a {
    color: #C25100;
    font-family: 'Lobster';
}

body {
    padding-left: 15px;
}
```

### try other fonts

https://fonts.google.com/

## CSS class

- `atom blog\templates\blog\post_list.html`

```
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href="//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext" rel="stylesheet" type="text/css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    {% for post in posts %}
                        <div class="post">
                            <div class="date">
                                <p>published: {{ post.published_date }}</p>
                            </div>
                            <h2><a href="">{{ post.title }}</a></h2>
                            <p>{{ post.text|linebreaksbr }}</p>
                        </div>
                    {% endfor %}
                </div>
            </div>
        </div>
        
    </body>
</html>
```

- `atom blog\static\css\blog.css`

```
.page-header {
    background-color: #C25100;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.content {
    margin-left: 40px;
}

h1, h2, h3, h4 {
    font-family: 'Lobster', cursive;
}

.date {
    color: #828282;
}

.save {
    float: right;
}

.post-form textarea, .post-form input {
    width: 100%;
}

.top-menu, .top-menu:hover, .top-menu:visited {
    color: #ffffff;
    float: right;
    font-size: 26pt;
    margin-right: 20px;
}

.post {
    margin-bottom: 70px;
}

.post h2 a, .post h2 a:visited {
    color: #000000;
}
```

## CSS self study

https://www.freecodecamp.org/learn/responsive-web-design/basic-css/

# Template の拡張

共通部分を別のファイルにして別のファイルにすることができます

- `atom blog\templates\blog\base.html`

```
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href="//fonts.googleapis.com/css?family=Bitter&subset=latin,latin-ext" rel="stylesheet" type="text/css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    {% block content %}
                    {% endblock %}
                </div>
            </div>
        </div>

    </body>
</html>
```

- `atom blog\templates\blog\post_list.html`

```
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h2><a href="">{{ post.title }}</a></h2>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock %}
```

# Application の拡張

- `atom blog\templates\blog\post_list.html`

```
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h2><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></h2>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock %}
```

- `atom blog\urls.py`

```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
]
```

- `atom blog\views.py`

```
from django.shortcuts import render
from django.utils import timezone
from .models import Post
from django.shortcuts import render, get_object_or_404

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})

def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})
```

- `atom blog\templates\blog\post_detail.html`

```
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        <h2>{{ post.title }}</h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```

# Blog の編集用 Form の作成

- `atom blog\forms.py`

```
from django import forms

from .models import Post

class PostForm(forms.ModelForm):

    class Meta:
        model = Post
        fields = ('title', 'text',)
```

- `atom blog\urls.py`

```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
    path('post/<int:pk>/edit/', views.post_edit, name='post_edit'),
    path('post/new/', views.post_new, name='post_new'),
]
```

- `atom blog\views.py`

```
from django.shortcuts import render
from django.shortcuts import render, get_object_or_404
from django.shortcuts import redirect
from django.utils import timezone
from .models import Post
from .forms import PostForm

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})

def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})

def post_new(request):
    if request.method == "POST":
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm()
    return render(request, 'blog/post_edit.html', {'form': form})

def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == "POST":
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_edit.html', {'form': form})
```

- `atom blog\templates\blog\base.html`

```
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href="//fonts.googleapis.com/css?family=Bitter&subset=latin,latin-ext" rel="stylesheet" type="text/css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    {% block content %}
                    {% endblock %}
                </div>
            </div>
        </div>

    </body>
</html>
```

- `atom blog\templates\blog\post_edit.html`

```
{% extends 'blog/base.html' %}

{% block content %}
    <h2>New post</h2>
    <form method="POST" class="post-form">{% csrf_token %}
        {{ form.as_p }}
        <button type="submit" class="save btn btn-default">Save</button>
    </form>
{% endblock %}
```

- `atom blog\templates\blog\post_detail.html`

```
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        <a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
        <h2>{{ post.title }}</h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```

## Security (へんしゅうボタンを管理者ユーザー以外に見せない）

- `atom blog\templates\blog\base.html`

```
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href="//fonts.googleapis.com/css?family=Bitter&subset=latin,latin-ext" rel="stylesheet" type="text/css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            {% if user.is_authenticated %}
                <a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
            {% endif %}
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    {% block content %}
                    {% endblock %}
                </div>
            </div>
        </div>

    </body>
</html>
```

- `atom blog\templates\blog\post_detail.html`

```
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        {% if user.is_authenticated %}
            <a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
        {% endif %}
        <h2>{{ post.title }}</h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```

### ブラウザのシークレット・モードで 127.0.0.1:8000 にアクセスして編集ボタンが表示されないことを確認
