# TODOアプリ作成で学んだことをメモ

## 初期設定

### プロジェクトの作成

``` bash
$ django-admin startproject todoproject .
```

### アプリの作成

``` bash
$ python manage.py startapp todo
```

### settings.pyの設定

#### templatesフォルダの指定

1. `templates` フォルダ作成
2. `TEMPLATES.DIRS` に `[BASE_DIR, 'template']` を書き込む。

##### 1. `templates` フォルダ作成

``` bash
$ ls 
manage.py  README.md  todo  todoproject
$ mkdir templates | ls
manage.py  README.md  templates  todo  todoproject
```

##### 2. `TEMPLATES.DIRS` に `[BASE_DIR, 'template']` を書き込む。

``` python settings.py

TEMPLATES = [

    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR, 'templates'],# ←追記箇所
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

``` 

#### 作成したアプリを参照できるように指定

`INSTALLED_APPS` に `todo` を追加

```python settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todo',#←追記箇所
]
```

### プロジェクトとアプリのurls.pyを繋げる

プロジェクトのurls.pyを編集
```python urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [

    path('admin/', admin.site.urls),
    path('', include('todo.urls')),

]

``` 

アプリ作成されたときに `urls.py` が存在しないので新規作成。

``` bash
$ touch todo/urls.py
```

## データベース周り

### model.pyの作成

アプリのmodels.pyでデータベースのテーブルに

```python models.py
from django.db import models

# Create your models here.

class TodoModel(models. Model):

    title = models.CharField(max_length=32)
    memo = models.TextField()

``` 

### マイグレーション

```bash 

$ python manage.py makemigrations
Migrations for 'todo':
  todo\migrations\0001_initial.py

    - Create model TodoModel

```

``` bash
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, todo
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
  Applying todo.0001_initial... OK
```

##　管理画面でデータを操作

### superuserの作成

``` 
$ python manage.py createsuperuser
Username (leave blank to use 'gakuy'): gakuy
Email address:
Password:
Password (again):
``` 

### 管理画面にてデータモデルを操作(ここはいらないかも)

アプリにあるadmin.pyを編集
```python adimn.py
from django.contrib import admin
from .models import TodoModel

# Register your models here.

admin.site.register(TodoModel)

```

##　データを一覧をリストとして表示する

## データの中身を表示する(DetailView)

`urls.py` でPKを決める

``` python urls.py
from django.urls import path
from .views import TodoList, TodoDetail

urlpatterns = [

    path('list/', TodoList.as_view()),
    path('detail/<int:pk>', TodoDetail.as_view())

]
```

## base.htmlを使用する

* Block header
* Block sidebar
* Block content
* Block footer

## createview

* fileds属性をviews.pyに追加しなければならない
* reverse_lazyについて

## Delete View

* 
