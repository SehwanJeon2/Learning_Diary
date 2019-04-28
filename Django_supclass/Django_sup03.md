> # 2019.04.25

[TOC]

# Django 보충 3일차

## 0. 디버그 툴 사용법



c9 전용

필수

```python
def show_toolbar(request):
    return True


DEBUG_TOOLBAR_CONFIG = {
    "SHOW_TOOLBAR_CALLBACK" : show_toolbar,
}

INTERNAL_IPS = ('0.0.0.0',)
```



---

## <시험대비 목표>

잊어버리기 쉽거나 난이도가 높은 로직 위주로 검토

* 회원가입
  * models, form 에서 차이 인지
  * import 경로
  * 함수 이름과 필요한 인자
  * User 커스터마이징
  * UserCreationForm 커스터마이징
* 로그인
  * import 경로
  * 함수 이름과 필요한 인자
* 로그아웃
  * import 경로
  * 함수 이름과 필요한 인자
* ManyToMany 다루기
  * models 에서 필요한 인자
  * views 에서 다루는 방법
* form 다루기
  * import 위치
  * meta class 말고 쓰는 것
* DatetimeField 다루기
  * 인자 2종류 익히기
  * CRUD 하는 방법 익히기
* 각종 import
  * models
  * form
  * urls
  * views
* admin
  * 그냥 띄우기
  * class로 띄우기
* 기타
  * json 삽입하기
  * csv 삽입하기



---

## 1. 새 프로젝트 생성

워크스페이스에서 새 폴더 생성 후 가상환경 설치

django 설치

```bash
$ mkdir FOREXAM
$ cd FOREXAM
$ pyenv virtual-env 3.6.7 forexam-venv
$ pyenv local forexam-venv
$ pip install django==2.1.8
$ pip install --upgrade pip

$ django-admin startproject monthlytest .
```



---

## 2. 새 앱 생성 및 설정

accounts, board 앱 생성

IPython은 shell 예쁘게 열게하기 위해서 걸치

```bash
$ python manage.py startapp accounts
$ python manage.py startapp board
$ pip install IPython
```



**settings.py**

ALLOWED_HOSTS 설정

앱 등록, 언어와 시간 설정 및 국제화 되었는지 확인.

```python
INSTALLED_APPS = [
    'accounts',
    'board',
]

LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```



**master urls.py**

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('board/', include('board.urls')),
    path('accounts/', include('accounts.urls')),
]
```



---

## 3. 회원가입

follower를 커스터마이징 할 것이 필요하다.

**board/models.py**



---

## 3. board

모델

charfield 최대 글자수 넘겨도 에러 메시지 출력 안한다.

대신에 잘라서 준다.

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=30)
    content = models.TextField()
    author = models.ForeignKey(setting.AUTH_USER_MODEL, on_delete=models.CASCADE, blank=True)
```



python 이 추구하는 철학 중 하나가 내장 베터리.



settings.py에서

TEMPLATES를 봐라.

c9은 비어있으나 Pycharm은 알아서 뭔가를 채워넣었다.

이게 뭘 불러오고 있는것이냐?





$ pip install IPython



**baord/models.py**

```python
from django.db import models
from django.conf import settings
from IPython imort embed

print(settings.AUTH_USER_MODEL)

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=30)
    content = models.TextField()
    # settings.AUTH_USER_MODEL == django.contrib.auth.models.User
    embed()  # 시간 멈추기
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)

```





앱의 urlpatterns 빈 껍데기라도 입력해야 문제없이 잘 돌아간다.

**baord/models.py**

```python
from django.db import models
from django.conf import settings

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=30)
    content = models.TextField()
    # settings.AUTH_USER_MODEL == django.contrib.auth.models.User
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    # author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    lovers = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='bookmarks')
```









---

## 로그인

??

login 폼은 2개의 인자를 받아야 한다. 요청 자체와 요청 내용중 POST.

로그인 안된 경우에만 로그인 가능하게.

```python
def login(request):
    if request.user.is_authenticated():  # 요청보낸 유저는 인증되었는가?
        return redirect('board:article_list')
        
    if request.method == "POST":
        form = AuthenticationForm(request, request.POST)
        if form.is_valid():  # 찾았냐?
            # 넵!
            auth_login(request, form.get_user())
        else:  # 아니오 ㅠㅠ
            return render(request, '', {
                'form': form,
            })
    else:
        form = AuthenticationForm()
        return render(request, '', {
            'form': form,
        })
```





## 로그아웃





## ?

```python
from django.views.decorators.http import require_GET, require_POST, require_http_methods
내가 겟만 받는다. post만 받는다.지정한 http만 가겠다.
@require_http_methods(['GET', 'POST']) get, post 요청만 받겠다.

@
```





---

로그인 한 사람만 글쓰기



```python
from django.contrib.auth.decorators import login_required

@login_required
def create
```









---



null=True 가 진정한 빈값을 저장할 수 있다고 하는 뜻이다

blank = True보다 더 빡세게 인정하는 것이라 함부로 쓰지 말라.

예를 들어 string이 blank=True에서는 '' 으로 저장. 하지만 null=True 는 진짜 null

