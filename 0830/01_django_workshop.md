## 1. intro/urls.py

intro/urls.py
dinner/<저녁메뉴>/<인원수>/ 형태의 요청 경로가 명시되는 파일

```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('dinner/<str:menu>/<int:people>/', views.dinner),
]
```

## 2. pages/views.py

Variable Routing의 개념을 활용한다.
저녁메뉴는 string, 인원수는 integer이다.

```python
from django.shortcuts import render

def dinner(request, menu, people):
    context = {
        'menu': menu, 
        'people': people,
    }
    return render(request, 'dinner.html', context)
```

## 3. templates/dinner.html

pages/views.py
Variable Routing을 통해 전달 받은 인자를 html 파일에서 사용할 수 있도록
렌더링 할 때 넘겨준다.
templates/dinner.html
html 마크업이 작성된 문서이다.
views.py에서 넘어온 데이터가 해당 문서에서 출력된다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>저녁 메뉴 추천</title>
</head>
<body>
  <h1>저녁 메뉴</h1>
  <h2>저녁 먹을 사람?! {{ people }} 명</h2>
  <h2>어떤 메뉴?! {{ menu }} </h2>
</body>
</html>
```
