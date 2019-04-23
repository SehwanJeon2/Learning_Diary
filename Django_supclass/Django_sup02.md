> # 2019.04.23

# Django 보충수업 2일차



이번주 금요일에 4월 월말평가는 출제가 완료됨.

코드 구현 3문제, 코드 일무를 준다. 고장난 코드가 아닌 미완성 코드가 제공.



지금 할거는 plus alpha가 메인, 르돔, 쿼리, 앱 있었다.

오늘까지는 django 목요일부터는 js



오늘 model 위주의 코드를 수업



end_game, accounts새 앱 생성

settings.py에 추가

끝이 아니라 맨 밑으로 가서

```python
AUTH_USER_MODEL = 'auth.User'
```



**endgame/models.py**

POST 클래스 생성

```python
from django.db import models
from django_extensions.db.models import TimeStampedModel
from django.conf. import settings

class Post(TimeStampedModel):
    # created, modified
    title = models.CharField(max_length=100)
    content = models.TextField()
```



User는 아이디, 유저네임, 패스워드,

Post에는 아이디, 타이틀, 컨텐트, user_id, 

User와 Post는 1대N.

from django.conf. import settings

는 settings 전체를 들고 옴.



auth_user의 db 열면 수많은 column이 있는 것을 볼 수 있다. 권한도 받을 수 있다.



settings.AUTH_USER_MODEL 는 그냥 문자열





pip freeze : 내가 쓰고있는 페키지의 버전을 출력. 사실 사람보라도 만든게 아님.

`freeze require > ` 

pip freeze > requirements.txt

pip install -r requirements.txt



jupyter 노트북에서



```shell
# create User
User.objects.create(
	username= 'edudu',
	password= '1234'
)
```



Post 객체 2개 생성한다.



```shell
Post.objcts.create(
	title = 'hihi',
	content ='비가 온답니다',
	user_id = 1
)
POST.objects.create(
	title="byebye",
	content = '내일은 보충이 없다'
	user_id = 2
)
```

유저 createion form을 쓰지 않고 shell을 통해서 유저 생성하면 비밀번호가 암호화 되지 않고 db에 저장된다. 



type(request.POST) 그냥 딕셔너리

```shell
from django.contrib.auth.forms import UserCreationForm

```





```shell
post = Post.objects.get(user_id=1) # 1번 유저가 작성한 post 중에 첫 번째 게시글
post.user.username
```

여기까지 1:N 

이제 Like를 구현해야 한다.

어떻게 2개의 table을 다대다로 연결할 것인가? 하나의 새로운 table을 생성하여 두 table을 이어주는 역할을 부여.

어떤 유저가 어떤 post를 좋아하는가를 



페이스북?? 에서 ... 누르면 광고 차단하기 기능이 들어있다. 차단 기능을 만들기 위해서도 위와 같이 새로운 table을 생성하여 써야한다.

즉 다대다를 사용하는 상황이 너무 많다는 것이다. 그래서 DB로 보면은 이걸 구분하기 힘든 모양이다. 하지만 models라면 좀 더 편하다.



유저는 작성한 post도 있고 좋아하는 post도 있다. 

현재 models 상황에서는 related_name이 필수다. 왜냐하면 post와 엮이는 상황이 2개라 post_set을 2번 써야하기 때문이다. 에러는 안 나겠지만, 둘 중 하나가 덮어씌워져서 못 쓸 것이다.



현재 이 시점에서 유저 2명 post도 2개 존재한다.



```shell
user = User.objects.get(id=1)
user.username

post = Post.objects.get(id=1)
post.content

# 둘 중 하나만 실행
# user가 post를 좋아한다
user.bookmarks.add(post)
# post를 user가 좋아한다
post.lovers.add(user)
```

둘 다 실행하면 에러는 안 내고, 아마 둘 중 하나가 덮어 씌워질 듯 하다.



endgame/forms.py

누굴 import를 해와야 하는지 외우어ㅑ 한다는게 불편

```python
from django import forms
from models. import Post

class PostForm(forms.ModelForm):
    title = forms.EmailField()
    
    class Meta:
        model = Post
        field = ['title', 'content', ]
```

클래스 Meta는 django 문법. Python과 전혀 상관 없다.

딴 얘기로 넘어가겠다는 뜻으로...?

views.py

```python
from django.shortcuts import render
from .models import Post
from django.conf import settings
from .forms import PostForm

# Create your views here.

def create(request):
    form = PostForm(request.POST)
    if form.is_valid():
        unsaved_post = form.save(commit=False)
        unsaved_post.user_id = 1
        unsaved_post.save()
    else:
        pass
```



사용자가 보내는 데이터는 세상에서 제일 믿을게 못된다.

그 데이터의 최종목적지는 DB.

날라온 데이터를 views가 먼저 받는다.

우리가 하나하나 다 맞는지 확인하고 save 시켜주기 싫다.

django는 models가 중간에 하나 더 둬서 해결

먼저, 넘어온 데이터를 묶어서 ModelForm으로 한 번 받은 뒤, 적합성을 따진다. 다만 적합성을 따지는 것은 자동으로 해주지 않기 때문에 우리가 명령어 하나 입력해야 한다. 그리고 검증 실패하면 되돌려보낸다.

`is_valid()` 가 검증 명령어

검증이 끝났으면 form.save() 를 통해서 post.save 기능으로 이어져서 POST 클래스에 객체 추가가 가능하다.

하지만 요소가 충분한지 아니면 뭐가 부족한지 모르는 상태로 바로 save까지 가야하나?? 중간에 멈추는게 필요하다.

commit False가 필요한 이유다. 

모델폼은 html까지 만들어준다.

forms.form / forms.modelform

근데 UserCrationForm 은 모델 폼이다.



우린 시험 잘보기 위해서면 views에서 이것들 잘 다루면 끝이다.

모델폼이 하는 일이 2가지라는 점을 잘 숙지하자. 





목요일 6시10분에 디버그 툴 사용법 알려주실것이다.



이거 연습해와바! 시험 잘 볼거임.

1. user 모델 쓰기(default) 바닥부터
2. post <-> user 묶기
   1. 하나는 작성자로써
   2. 좋아요 누른 사람으로써
3. 회원가입





MEDIA_ROOT (정확하게 media 폴더를 가리키는 경로)

os.path.join 며령어 거치면 아래와 같이 대충 출력될거다

sf/sfese/sdfew/sdfes/MEDIA



MEDIA_URL : 우리가 img 태그ㅔ src 쓸때 /media/를 캐치하는 것



urls.py에 추가하는 이유는

순전히 우리가 개발 중이기 때문이다. settings에서 debut True이면

static 함수의 리턴값이 string으로 ['...../media/'] 라고 되어있다.

장고가 이미지 서빙할 때, degug가 false일 때 제대로 못 한다.



c9이 서버 열어준거지 서버를 베포한다고 하진 않느다.

배포하면 여러개로 쪼개진 css 파일들이 하나로 합쳐진다.

실제로 bootstrap은 2개의 css 링크를 준다. 하나는 개발용, 하나는 베포용. 베포용은 6000줄 짜리 css를 전부 1줄로 표현. 엔터키 하나하나도 다 불필요한 데이터다.

베포랑 개발은 정말 딴 세게다. 파일 이름도 지 마음데로 다 바꿔버림.



