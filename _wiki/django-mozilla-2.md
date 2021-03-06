---
layout  : wiki
title   : django-mozilla-2 
summary : 
date    : 2020-02-02 21:52:52 +0900
updated : 2020-02-03 18:57:50 +0900
tags    : django
toc     : true
public  : true
parent  : python
latex   : false
---
* TOC
{:toc}

> [Moziila djangdo tutorial](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Introduction) 읽으며 정리한 내용입니다. 원본을 보시길 추천드립니다. 지적과 피드백 대환영입니다.

여기 [Github](https://github.com/mdn/django-locallibrary-tutorial)에 완전히 개발된 버전의 웹사이트 소스코드를 참고할 수도 있습니다.

- [장고 개발환경 세팅](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/development_environment)
- [튜토리얼 개요](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Tutorial_local_library_website)

## Overview

1. **`django-admin`** - 프로젝트 폴더, basic file templates, 그리고 프로젝트 관리 스크립트 (**manage.py**)를 만들기 위해서 `django-admin` 을 사용
2. **manage.py** - 애플리케이션을 만들기 위해서 **manage.py** 를 사용(장고는 필요할 때 다른 프로젝트에서 재사용이 가능하도록 분리된 애플리케이션으로 개발하는 것을 추천)
3. 프로젝트에 포함시키기 위해 새 애플리케이션들을 등록(register)
4. 각 애플리케이션에 대해 url/mapper를 연결(hook up)

## catalog application 만들기
```python
$ mkdir locallibrary 
$ cd locallibrary

$ pipenv --three

$ pipenv shell

$ pipenv install Django

$ django-admin startproject locallibrary

$ cd locallibrary
``` 

```
locallibrary/
    manage.py
    locallibrary/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

* **init.py** — 장고/파이썬이 폴더를 Python Package 로 인식하게 할 빈 파일. 또한 프로젝트의 다른 부분에서 객체(object)를 사용할 수 있게 한다.
* **settings.py**  - 웹사이트의 모든 설정을 포함. 어떤 application이라도 등록이 되는 곳이며,  static files의 위치, database 세부 설정 등이 들어간다.
* **urls.py** - 사이트의 url - view의 연결을 지정. 모든 url 매핑 코드가 포함될 수 있지만 특정한 애플리케이션에 매핑의 일부를 할당해주는 것이 일반적.
* **wsgi.py** - 장고 어플리케이션이 웹서버와 연결 및 소통하는 것을 돕는다. 표준 형식(boilerplate)으로 다뤄도 무방.

## catalog application 만들기
```
    $ python3 manage.py startapp catalog
```

```
locallibrary/
    manage.py
    locallibrary/
    catalog/
        admin.py
        apps.py
        models.py
        tests.py
        views.py
        __init__.py
        migrations/
```

## catalog application 등록하기

```python
## filename:  locallibrary/locallibrary/settings.py 

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        **'catalog.apps.CatalogConfig',** 
    ]
```

새로운 행은 애플리케이션 구성 객체(application configuration object) (CatalogConfig)를 지정. 애플리케이션을 생성할 때 /locallibrary/catalog/apps.py 안에 생성.

> '앱 이름.apps.앱 이름+Config'

## Database 설정하기

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
이 예제에서는 SQLite 데이터베이스 사용. 많은 동시 접속을 예상하지 않고 설정에 추가적인 작업이 필요없기 때문. `settings.py`에서 확인. SQLite를 사용하므로 다른 어떤 작업을 할 필요가 없음. [Databases](https://docs.djangoproject.com/en/3.0/ref/databases/)에서 가능한 다른 옵션을 확인할 수 있음.

## 프로젝트의 다른 설정

```
## filename: settings.py

    TIME_ZONE = 'Asia/Seoul'
```

지금은 바꾸지 않지만 알아둬야 할 두 가지 설정이 있다.

- `SECRET_KEY` - 보안 전략의 일부로 사용되는 비밀 키. 만약 이 코드를 개발 과정에서 보호하지 않는다면 제품화(production) 과정에서 다른 코드(아마 환경 변수나 파일에서 읽어오는)를 사용해야 할 것.

- `DEBUG` -  에러가 발생했을 때 HTTP 상태 코드 응답 대신 디버깅 로그를 표시하게 만듦. 디버깅 정보는 공격자에게 유용하기 때문에 제품화된(production) 환경에서는 `False` 로 설정해야 한다. 하지만, 지금은 `True`로 설정.

## URL 매퍼 연결하기

```python
## filename: locallibrary/urls.py

    from django.contrib import admin
    from django.urls import path
    from django.conf.urls import include
    from django.views.generic import RedirectView
    from django.conf import settings
    from django.conf.urls.static import static
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('catalog/', include('catalog.urls')),
        path('', RedirectView.as_view(url='/catalog/', permanent=True)),
    ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```
- urlpatterns 리스트는 맨 처음에 관리자 어플리케이션의 고유한 URL 매핑 정의를 갖고 있는 `admin.site.urls` 모듈에 `admin/` 패턴을 가지고 있는 모든 URL을 매핑하는 단일 함수를 정의.

```python
urlpatterns += [
    path('catalog/', include('catalog.urls')),
]
```
- 요청(request)을 모듈 `catalog.urls`(관련 URL /catalog/urls.py가 있는 파일) 에  `catalog/`  패턴과 함께 전달하는 `path()`를 포함. ~~뭔 소리??~~(번역자주: 만일 `www.xxxx.com/catalog` 로 시작되는 request가 오면 `catalog/urls.py`를 참조해서 관련된 파일을 mapping 하겠다는 의미) 

```python
urlpatterns += [
    path('', RedirectView.as_view(url='/catalog/', permanent=True)),
]
```

- 뷰 함수(RedirectView)는 `path()`에 지정된 URL 패턴이 일치할 때(위의 경우에선 루트 URL) 첫 번째 인자를 `(/catalog/)`로 리다이렉트

```python
# Use static() to add url mapping to serve static files during development (only)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

- 장고는 기본적으로 CSS, JavaScript, 그리고 이미지와 같은 정적 파일을 제공하지 않지만, 이들은 사이트에 매우 유용할 수 있다. 최종적으로 이 URL 매퍼에 추가할 것은 개발 중에 정적 파일들을 제공하는 것을 가능하게 하는 코드다.

```python
from django.urls import path
from catalog import views


urlpatterns = [

]
```

마지막으로 `urls.py`라는 파일을 catalog 폴더 안에 생성. 그리고 임포트된 텅 빈 urlpatterns를 정의하기 위해 아래 코드를 추가해야 한다. 어플리케이션을 만들면서 패턴들을 이곳에 추가할 것이다. 

## Website framework 테스트 하기
```
$ pythons manage.py makemigrations
$ pythons manage.py migrate
$ pythons manage.py runserver
```

---

> url 이해가 잘 안되어서 아래는 [장고 공식문서](https://docs.djangoproject.com/en/3.0/topics/http/urls/)에서 옮겼는데 위와 관련이 없는 내용인 것 같습니다. 스킵하셔도 좋습니다.

## Example

```python
## filename: sample URLconf

from django.urls import path

from . import views

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
]
```

* URL 값을 캡처하기 위해 ()를 쓴다.
* 캡처된 값은 converter type을 선택적으로 include할 수 있다. 이를테면 정수 파라미터를 캡처하기 위해 `<int:name>`를 쓴다. 만약 converter가 포함되지 않는다면 어떤 스트링(슬래시를 제외한)도 사용 가능하다.
* 맨 앞에 슬래시를 붙일 필요는 없다. 예를 들어 `articles`로 쓰지 `/articles`로 쓰지 않는다.

**Example requests**
* `/articles/2005/03/`은 위 리스트이 3번째줄에 위치한 코드에 매치된다. 장고는 `views.month_archive(request, year=2005, month=3)` 함수를 호출한다.
* `/articles/2003/`은 위 리스트 2번째줄이 아닌 1번째줄에 위치한 코드와 매치된다. 순서대로 테스트되는 패턴이기 때문이다.
* `/articles/2003`은 이 패턴들 어떤 것과도 매치되지 않는다. 각 패턴들은 모두 URL 끝에 슬래시를 요청하는 이유다.
* `/articles/2003/03/building-a-django-site/`는 마지막 패턴과 매치된다.장고는 `views.article_detail(request, year=2003, month=3, slug="building-a-django-site")`를 호출할 것이다.
 
### Path converters
다음의 path conveter들은 디폴트로 쓸 수 있다.
* **str** - 경로를 나누는 슬래시를 제외하고 어떤 문자열도 매치된다. 표현식 converter가 없으면 strtind이 디폴트로 설정된다.
* **int** - 0 내지 양의 정수가 매치된다. int를 리턴.
* **slug** - 아스키 문자의 slug 문자나 숫자, 하이픈, 언더스코어가 매치된다. 예를 들어 `building-your-1st-django-site`.
* **uuid** - 같은 페이지 내 매핑된 다중 URL들을 막기 위해 대시와 소문자로 포함된다. 예를 들어 075194d3-6885-417e-a8a8-6c931e272f00.

[Django Tutorial Part 3: Using models](https://yongjunleeme.github.io/wiki/django-mozilla-3/)

## Link
[장고 튜토리얼 2]([https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/skeleton_website](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/skeleton_website))
[장고 공식문서](https://docs.djangoproject.com/en/3.0/topics/http/urls/)

