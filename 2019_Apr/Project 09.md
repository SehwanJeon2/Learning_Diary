# Project 09

## 1. 데이터베이스 설계

### Movie

**movies/models.py**

```python
class Movie(models.Model):
    title = models.CharField(max_length=30)
    audience = models.IntegerField()
    poster_url = models.TextField()
    description = models.TextField()
    genre = models.ForeignKey(Genre, on_delete=models.CASCADE)
```



### Genre

**movies/models.py**

```python
class Genre(models.Model):
    name = models.CharField(max_length=20)
```



### Score

**movies/models.py**

```python
class Score(models.Model):
    content = models.CharField(max_length=50)
    value = models.IntegerField()
    movie = models.ForeignKey(Movie, on_delete=models.CASCADE)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
```



### accounts_user_followers

**accounts/models.py - User**

새로운 table 생성하지 않고 기존의 User table에 추가했다.

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.conf import settings

# Create your models here.
class User(AbstractUser):
    to_user = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name="from_user")

```



---

## 2. Seed Data 반영

### 데이터 주입

주어진 `movie.json` , `genre.json` 을 아래 명령어를 통해 반영

```bash
$ python manage.py loaddata genre.json
Installed 11 object(s) from 1 fixture(s)
$ python manage.py loaddata movie.json
Installed 10 object(s) from 1 fixture(s)
```



### admin으로 확인

**movies/admin.py**

```python
from django.contrib import admin
from .models import Movie, Genre

# Register your models here.
admin.site.register(Movie)
admin.site.register(Genre)
```



---

## 3. accounts App



