---
title: "Django initial setting"
date: "2019-11-11T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Django_initial_setting"
category: "기본 개념"
tags:
  - "Restful API"  
description: "Django Restful API 개발(2) - 기본 개념"
socialImage: ""
---


## secrets_gen.py
access token 만들 때 사용할 SECRET_KEY 생성  
print된 SECRET_KEY를 my_settings.py 파일에 추가해주면 된다.

```python
import string
import random
​
# Get ascii Characters numbers and punctuation (minus quote characters as they could terminate string).
chars = ''.join([string.ascii_letters, string.digits, string.punctuation]).replace('\'', '').replace('"', '').replace('\\', '')
​
SECRET_KEY = ''.join([random.SystemRandom().choice(chars) for i in range(50)])
​
print(SECRET_KEY)
```


## my_settings.py
```python
from datetime import datetime, timedelta
​
DATABASES = {
    'default' : {
        'ENGINE': 'mysql.connector.django',
        'NAME': 'soogo',
        'USER': 'root',
        'PASSWORD': 'pass1423',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
​
SOOGO_SECRET = {
        'secret':'j.>#ZM3ebFZfHbT4.Sa31V$R</k~S7#2gzL)Ow>5C2!?]k!A+I',
        'exp_time': datetime.now() + timedelta(seconds = 60 * 60 * 24),
}

```

## settings.py
```python
"""
Django settings for soogo project.
​
Generated by 'django-admin startproject' using Django 2.2.2.
​
For more information on this file, see
https://docs.djangoproject.com/en/2.2/topics/settings/
​
For the full list of settings and their values, see
https://docs.djangoproject.com/en/2.2/ref/settings/
"""
​
import os
import my_settings
​
# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
​
​
# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/2.2/howto/deployment/checklist/
​
# SECURITY WARNING: keep the secret key used in production secret!
#SECRET_KEY = 'i(d(w4ab_hxe2zvm383k61572=mlg&b^&_py%8%s)!ouaw3-0j'
SECRET_KEY = my_settings.SOOGO_SECRET['secret']
EXP_TIME = my_settings.SOOGO_SECRET['exp_time']
​
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True
​
ALLOWED_HOSTS = ['*', 'localhost', 'localhost:8000', '10.58.3.239:8000', '10.58.3.239', '127.0.0.1']
​
# Application definition
​
INSTALLED_APPS = [
   # 'django.contrib.admin',
   # 'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'corsheaders',
    'user',
    'product',
]
​
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
   # 'user.middleware.DisableCsrfCheck',
    'corsheaders.middleware.CorsMiddleware',
    #'django.middleware.csrf.CsrfViewMiddleware',
    #'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
​
ROOT_URLCONF = 'soogo.urls'
​
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
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
​
WSGI_APPLICATION = 'soogo.wsgi.application'
​
​
# Database
# https://docs.djangoproject.com/en/2.2/ref/settings/#databases
​
DATABASES = my_settings.DATABASES
​
​
# Password validation
# https://docs.djangoproject.com/en/2.2/ref/settings/#auth-password-validators
​
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]
​
​
# Internationalization
# https://docs.djangoproject.com/en/2.2/topics/i18n/
​
LANGUAGE_CODE = 'en-us'
​
TIME_ZONE = 'Asia/Seoul'
​
USE_I18N = True
​
USE_L10N = True
​
USE_TZ = True
​
​
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.2/howto/static-files/
​
STATIC_URL = '/static/'
​
​
##Stop Warning about '/'
APPEND_SLASH=False
​
##CORS
CORS_ORIGIN_ALLOW_ALL=True
CORS_ALLOW_CREDENTIALS = True
​
CORS_ALLOW_METHODS = (
    'DELETE',
    'GET',
    'OPTIONS',
    'PATCH',
    'POST',
    'PUT',
)
​
CORS_ALLOW_HEADERS = (
    'accept',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
)
```

## requirement.txt
project에 필요한 프로그램 목록을 표시해준다.
```python
bcrypt==3.1.7
beautifulsoup4==4.8.1
bs4==0.0.1
certifi==2019.9.11
cffi==1.13.1
chardet==3.0.4
Django==2.2.6
django-cors-headers==3.1.1
idna==2.8
mysql-connector-python==8.0.18
protobuf==3.10.0
pycparser==2.19
PyJWT==1.7.1
pytz==2019.3
requests==2.22.0
selenium==3.141.0
six==1.12.0
soupsieve==1.9.4
sqlparse==0.3.0
urllib3==1.25.6
```

## .gitignore
my_settings.py Database password, Secret_key와 같은 파일은 공개되면 안되는 정보를 포함하고 있기 때문에 .gitignore에 등록해야 된다. 
```python
#my_settings.py
my_settings.py

#parser.py
parser.py

# Created by https://www.gitignore.io/api/vim,macos,python,django
# Edit at https://www.gitignore.io/?templates=vim,macos,python,django

### Django ###
*.log
*.pot
*.pyc
__pycache__/
local_settings.py
db.sqlite3
db.sqlite3-journal
media

# If your build process includes running collectstatic, then you probably don't need or want to include staticfiles/
# in your Git repository. Update and uncomment the following line accordingly.
# <django-project-name>/staticfiles/

### Django.Python Stack ###
# Byte-compiled / optimized / DLL files
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
pip-wheel-metadata/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
.hypothesis/
.pytest_cache/

# Translations
*.mo

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# pyenv
.python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# celery beat schedule file
celerybeat-schedule

# SageMath parsed files
*.sage.py

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# Mr Developer
.mr.developer.cfg
.project
.pydevproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/

### macOS ###
# General
.DS_Store
.AppleDouble
.LSOverride

# Icon must end with two \r
Icon

# Thumbnails
._*

# Files that might appear in the root of a volume
.DocumentRevisions-V100
.fseventsd
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns
.com.apple.timemachine.donotpresent

# Directories potentially created on remote AFP share
.AppleDB
.AppleDesktop
Network Trash Folder
Temporary Items
.apdisk

### Python ###
# Byte-compiled / optimized / DLL files

# C extensions

# Distribution / packaging

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.

# Installer logs

# Unit test / coverage reports

# Translations

# Scrapy stuff:

# Sphinx documentation

# PyBuilder

# pyenv

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.

# celery beat schedule file

# SageMath parsed files

# Spyder project settings

# Rope project settings

# Mr Developer

# mkdocs documentation

# mypy

# Pyre type checker

### Vim ###
# Swap
[._]*.s[a-v][a-z]
[._]*.sw[a-p]
[._]s[a-rt-v][a-z]
[._]ss[a-gi-z]
[._]sw[a-p]

# Session
Session.vim
Sessionx.vim

# Temporary
.netrwhist
*~
# Auto-generated tag files
tags
# Persistent undo
[._]*.un~

# End of https://www.gitignore.io/api/vim,macos,python,django
```

## Mysql
+ MySQL 5.7 버젼 설치
+ MySQL 클라이언트(sequelpro) 설치
+ MySQL에 접속해서 database 생성
```python
CREATE DATABASE mydatabase CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci
```
+ mysql-connector-python 라이브러리 설치
MySQL을 파이썬에서 사용하기 위함이다.
```python
pip install mysql-connector-python
```
+ Django에서 데이터베이스 설정은 settings.py 파일에 DATABASES 변수를 다음과 같이 바꿔주면 된다.
```python
DATABASES = {
    'default': {
        'ENGINE': 'mysql.connector.django', 
        'NAME': 'DB_NAME',
        'USER': 'DB_USER',
        'PASSWORD': 'DB_PASSWORD',
        'HOST': 'localhost',   # Or an IP Address that your DB is hosted on
        'PORT': '3306',
    }
}
```
my_settings.py에서 이미 설정을 해줬기 때문에 아래와 같이 해주면 된다.
```python
DATABASES = my_settings.DATABASES
```

## AWS