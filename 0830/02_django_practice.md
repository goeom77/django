## ❖ Django Project

### intro/는 startproject 명령어로 생성되는 project 디렉토리이다.

1. 가상환경 설정
   가상환경 생성 -> 가상환경 실행 -> `pip install django==3.2 requests` 

2. 프로젝트 생성

3. 어플리케이션 생성

4. `settings.py` 에 어플리케이션 등록

5. urls.py 작성

6. views.py 작성

7. template 작성
* `settings.py` 에 어플리케이션 등록 및 언어/시간 설정

```python
# intro/settings.py

INSTALLED_APPS = [
    'pages',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

...
LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```

* `urls.py` 작성

```python
# intro/urls.py

from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('lotto/', views.lotto),
    path('admin/', admin.site.urls),
]
```

* `views.py` 작성

```python
# pages/views.py

from django.shortcuts import render
import requests
import random

def lotto(request):
    API_URL = 'https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=1'
    # 1. 이번 회차 당첨 정보 받아오기
    lotto = requests.get(API_URL).json()

    # 2. 번호 6개만 가져오기
    winner = []
    bonus = lotto['bnusNo']
    for i in range(1, 7):
        winner.append(lotto[f'drwtNo{i}'])

    # 3. 당첨 횟수 기록하기 위한 dictionary 만들기
    win_rate = {'1등': 0, '2등': 0, '3등': 0, '4등': 0, '5등': 0, '꽝': 0}

    # 4. 로또 1000장 구매하고 각 당첨번호와 비교하기
    for i in range(1000):
        my_numbers = random.sample(range(1, 46), 6)

        # 5-1. 내 자동으로 산 복권 번호와 당첨번호 반복문으로 개수 비교
        matched = 0
        for num in my_numbers:
            if num in winner:
                matched += 1

        # 5-2. 교집합으로 개수 비교
        # matched = len(set(winner) & set(my_numbers))

        # 6. 당첨된 횟수 체크
        if matched == 6:
            win_rate['1등'] += 1
        elif matched == 5 and bonus in my_numbers:
            win_rate['2등'] += 1
        elif matched == 5:
            win_rate['3등'] += 1
        elif matched == 4:
            win_rate['4등'] += 1
        elif matched == 3:
            win_rate['5등'] += 1
        else:
            win_rate['꽝'] += 1

    context = {
        'winner': winner,
        'bonus': bonus,
        'win_rate': win_rate,
    }

    return render(request, 'lotto.html', context)
```

* `lotto.html` 작성
  
  ```html
  <!-- templates/lotto.html -->
  ```

<!DOCTYPE html>

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>로또 당첨 횟수를 알아보자.</h1>
  <hr>
  <h2>이번 회차 당첨 번호 : {{ winner }} + {{ bonus }}</h2>
  <ul>
    {% for key, val in win_rate.items %}
      <li>{{ key }} : {{ val }}번</li>
    {% endfor %}
  </ul>
</body>
</html>
```