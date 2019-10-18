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

- edit blog\models.py

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

- edit blog\admin.py

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
   + .gitignore というファイルを作成する。これらのファイルは add, commit の対象にならない
```
*.pyc
*~
__pycache__
myvenv
db.sqlite3
/static
.DS_Store
```

   + `git add --all .`
   + `git commit -m "My Django Girls app, first commit"`
   + `git remote add origin https://github.com/<your-github-username>/my-first-blog.git`
   + `git push -u origin master`
- Create PythonAnywhere account
  + https://www.pythonanywhere.com にアクセスして User ID を作成する。無料の Beginner をえらぶ。User 名は URL のいちぶになるので良い名前をえらぶ
  + pythonanywhere でトークンをさくせい。Account -> API Token -> Create a new API Token
  + pythonanywhere の画面で bash をうごかして以下のコマンドをにゅうりょく。（プロジェクトごとに1回だけ）
     - pip3.6 install --user pythonanywhere
     - pa_autoconfigure_django.py https://github.com/your-github-username/my-first-blog.git (your-github-username を自分の ID におきかえる)
     - python manage.py createsuperuser
  + ユーザID.pythonanywhere.com にアクセスしてうごいていることをかくにん
     
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
