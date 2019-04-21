> # 2019.04.18

# Django 보충수업 01

## 1. 개요

DB 관계를 그림으로 그린 경우.

사람말로 설명하는 것과 다르게 읽도록 해야 한다.

특히 1:N 같은 경우 내용물을 채우기 전과 후에 사람이 그림을 읽을 수 있는 정도가 달라짐.

존이 하이라는 글을 썻고 거기엔 여러 댓글이 달릴 수 있는데 톰이 달렸어~~

RDBMS <=> NSQL

DB 를 sql 없이 조작하도록 하는 것이 ORM

결국엔 관계(R) 이 핵심. 이게 형성되기 전에는 사람도 DB의 의미를 읽기 힘들어한다.

레코드, column이 많아서라 아니라 관계를 표현할 수 있다는게 핵심.





look up 이란 단어 들어봤나?

쿼리 = 질의문, 근데  To who? DB에게 묻는 질의문.

사람과 다르게 컴퓨터는 인삿말도 맘데로 짧게, 혹은 예의 차리고 할 수 없다. 통일 된 언어를 사용해야 한다. 사실상 통일될 필요 없다면 인공지능이 완성되었다고 할 수 있다.

쿼리는 구조화된 질의문을 한다고 보는게 더 정확. 왜 이게 언급되는가? ... 나중에



처음엔 sql로 DB 조작하고 했다. 그러다 ORM이라는 신세계가 등장.



ORM과 ORM 쓸 때 python 언어를 쓴다. sql과 python은 다른 세계에서 산다. 그걸 ORM이 중계자.



---

## 2. 편의기능 설치 및 세팅

c9 워크스페이스 jsws_practice에서 진행.

원래는 sup_class를 만들어 두었으나, python 설치 시간 때문에 기존 걸 사용.



Django 너무 높은 버전 깔려 있으니, 2.1.7 로 바꾸자.

```bash
$ pip uninstall django && pip install django==2.1.7
```



Django 편의기능과 ipython notebook을 설치

```bash
$ pip install django-extensions 'ipython[notebook]'
```



**settings.py**

pycharm을 사용하는 사람들은 shift 2번으로 편리하게 사용가능.

settings에 장고 익스텐션 추가

```python
INSTALLED_APPS = [
    'django_extensions',
    ...
]
```



**디버그 툴바 설치**

지금은 새로온 사람들 때문에 복습하고 있지만, 이전부터 보충 수업 듣고 있는 사람들을 위한 선물.

디버그를 편하게 해주는 프로그램인데 나중에 사용할 예정.

```bash
$ pip install django-debug-toolbar
```



**settings.py**

디버그 툴바는 MIDDLEWARE에 등록해야 한다.

`INTERNAL_IPS` 는 서버를 어떤 주소로 돌려야 겠는지 정하는 것이다. 아래에 입력한 번호는 내 로컬로 서버를 돌리겠다는 뜻으로, 모든 컴퓨터의 공통 요소다.

```python
ALLOWED_HOSTS = ['*']

MIDDLEWARE = [
    ...,
    'debug_toolbar.middleware.DebugToolbarMiddleware',
]
...
INTERNAL_IPS = ('127.0.0.1', )
# 내 로컬로 서버를 돌리겠다. 모든 컴퓨터의 공통.
```



**마스터 urls.py**

사람마다 맨 위 상단의 urls를 부르는 명칭이 다르다.

강동주 쌤 - 루트 url

3반 쌤 - 마스터 url

디버르 툴바를 사용하기 위해서는 마스터 urls.py에 등록해야 한다.

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings

urlpatterns = [
    path('admin/', admin.site.urls),
]

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        path('__debug__/', include(debug_toolbar.urls)),
    ] + urlpatterns
```



**기타 이야기**

개발자모드에서 네트워크 속도 조절 가능.



인터넷 속도가 빨라도 응답이 늦으면 인터넷이 느려 보인다.



쓰로트로잉을 온라인으로 설정 바꿔라. 안그러면 속 터진다.

intercept redirect는 뭔가?

return redirect 할 때 바로 새로고침하지 말고 그 바로전에 멈처서 중간단계 검토할 때 사용.



shell_plus 인터넷 창으로 jupyter notebook 형식으로 돌리기

아래 코드는 오직 local computer 에서만 돌아간다.

```bash
$ python manage.py shell_plus --notebook
```



저번 시간엔 settings.py 에서 `INSTALLED_APP` 목록 중 ledom 을 추가했었는데 지금은 안 쓰므로 주석처리.



---

## 3. 앱 생성 및 models 입력

**bash로 장고버전만 체크**

```bash
$ pip list | grep Django
Django               2.1.8  
```



**쿼리 앱 생성**

```bash
$ python manage.py startapp query
```



**query/models.py - 3개 클래스**

User, Post, Comment

3개를 작성

User : 이름을 charfield로 저장.

POST : 게시글 제목과 유저 저장.

Comment : 내용과 유저 id, 게시글 id 저장.

```python
from django.db import models
from django_extensions.db.models import TimeStampedModel

# Create your models here.
class User(TimeStampedModel):
    name = models.CharField(max_length=20, unique=True)

class Post(TimeStampedModel):
    title = models.CharField(max_length=20, unique=True)
    user = models.ForeignKey(User, on_delete=models.CASCADE)

class Comment(TimeStampedModel):
    content = models.CharField(max_length=20, unique=True)
    post = models.ForeignKey(Post, on_delete=models.CASCADE)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
```



**`CharField()` vs `TextField()`**

CharField, TextField는 둘 다 상관없다. 그냥 습관 따라.

성능상 차이는 없다고 밝혀졌지만, 데이터 구조를 만들 때, 고민하게 된다.

TextField도 무한하게 만들지 않았다.



하지만 `is_valid()` 처럼 데이터 검증 때 CharField가 더 편한 면이 있다.



mysql이든 oracle이든 RDBMS는 

ORM은 어떤 DB를 쓰든 알아서 다 번역 해준다.

그 나머지는 통일된 언어로 할 수 있다. 그래서 각각 의 DB 전문가들이 필요없고 그냥 ORM 1개만 잘 다루면 어떤 DB던 잘 다룰 수 있다. 걔네들이 sql 언어로는 다 로직이 다를 수 있는데 정말 다행이다.





TimeStampedModel

models.py

```python
from django.db import models
from django_extensions.db.models import TimeStampedModel

# Create your models here.
class User(TimeStampedModel):
    name = models.CharField(max_length=20, unique=True)


class Post(TimeStampedModel):
    title = models.CharField(max_length=20, unique=True)
    user = models.ForeignKey(User, on_delete=models.CASCADE)


class Comment(TimeStampedModel):
    content = models.CharField(max_length=20, unique=True)
    post = models.ForeignKey(Post, on_delete=models.CASCADE)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
```



---

## 4. makemigrations & migrate

**makemigrations & migrate**

migrations이 안되서 왜 안되나 봤더니

이걸로 app 등록이 안되어있다는 것을 알 수 있었다.

bash로 특정 앱만 `makemigrations` 시킬 수 있다.

```bash
$ python manage.py makemigrations query
App 'query' could not be found. Is it in INSTALLED_APPS?
```



**migrations 역할**

당장 이 코드만으로는 죽었다 깨어나도  DB 세상으로 갈 수 없다. 하지만 ORM의 도움을 받아서 저세상으로 갈 수 있다.

(ORM도 세상에 많이 존재하지만 우린 그 중에서 Django ORM을 사용하고 있는 것이다.)

즉 이걸 db가 알아듣도록 번역하는 과정이 필요하다. 하지만 그 중간에 제3python 언어로 설계도를 그려야 한다. 그게 makemigrations. 이거 까봐도 한 번도 본 적 없는 코드만 나온다. 근데 이게 바로 sql로 번역할 준비를 마춘 상태라고 볼 수 있다.

migrate하면, 그 읽기 힘든 설계도를 확실하게 전달, 실행해준다.



우리의 수많은 요구사항을 삼성에 직접 전달할 순 없음. 프로님이 볼 수 있게 요구사항을 정리하는게 migration. 프로님이 이 요구사항들을 보고서 형식으로 올리는게 migrate.

만약 요구사항이 변경되었다면???

보고서를 처음부터 작성하지 않고, 기존에서 바뀐 것만 수정해서 새로운 보고서를 만든다.

makemigrations 다시 하면 0002 파일 생성.

```python
class User(TimeStampedModel):
    name = models.CharField(max_length=20, unique=True)
    email = models.EmailField(blank=True)
```



직접 뜯어보니 아주 간단한 함수만 더 추가 되어있다. email이 추가 됐다는 내용으로 보인다.

하지만 migrate를 해야만 실제로 DB에 저장한다.



email 삭제해보자.

migrations 파일 제목 보면 사소한 변경사항이 제목으로 반영된걸 볼 수 있다.

0001_initial

0002_user_email

0003_remove_user_email

이렇게 만든 이유는 순서를 지키기 위해서. 아무순서데로 진행하다간 큰일 나므로.



그럼 만약 아래와 같은 코드를 추가하면?

```python
def __str__(self):
```

위 코드는 DB에 영향을 미치는 코드가 아니다. 그래서 makemigrations 해도 달라질 게 없다.

그래도 자신이 이걸 구분할 자신이 없으면 항상 migrations 해보자.



---

## 5. Shell_plus로 DB 다루기

실제 DB에 뭔가를 추가하는 과정을 해보자.

수업은 로컬에서 하기에 jupyter notebook으로 작업.

나는 c9이라서 그냥 shell_plus



```shell
user = User.objects.create(name="싸피")

user = User.objects.create(name="싸피") 
fail ... 
unique 어쩌구 저쩌구...
```

jupyter note 특성상 이미 실행된 코드가 반복 실행되기 때문에 위와 같은 문제가 자주 생김. (shift + Enter 는 코드를 실행하면서 내려가기 때문)

그래서 이미 실행한 코드는 주석처리하는 습관을 키워야 한다.



```shell
In [4]: User
Out[4]: query.models.User

In [2]: user
Out[2]: <User: User object (1)>

In [3]: user.name
Out[3]: '싸피'

In [8]: User.objects.get(id=1)
Out[8]: <User: User object (1)>
```



modles.py

```python
class User(TimeStampedModel):
    name = models.CharField(max_length=20, unique=True)
    
    def __str__(self):
        return self.name
```



```shell
In [1]: User.objects.get(pk=1)
Out[1]: <User: 싸피>
```





```shell
In [3]: post = Post.objects.create(title="안녕하세요", user_id=User.objects.get(pk=1).id) 

In [4]: post 
Out[4]: <Post: Post object (1)
```

create 대신에 post.save() 도 가능.

```shell
In [6]: post = Post.objects.create(title="안녕히가세요", user=user)

In [7]: post                                                                           
Out[7]: <Post: Post object (2)>
```



왜 객체, id를 둘 다 받을 수 있는가?

처음에는 DB에 넣을 수 있는게 없다. 숫자 혹은 boolean 뿐...

DB의 한계 때문에???

orm이 알아서 객체로부터 id를 뽑아서 넣을 수 있다.



---

## 6. 시험에 관한 팁

다음 4가지 코드 중에서 기능이 다른것은??





자료 받고 집에 귀가하기~!



zzu.li/

