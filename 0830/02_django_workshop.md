```python
from django.contrib import admin
from django.urls import path
from pages import views

urlpatterns = [
    path('lotto/', views.lotto),
    path('admin/', admin.site.urls),
]
```

```python
import random
from django.shortcuts import render

# Create your views here.
def lotto(request):
    numbers = random.sample(range(1, 46), 6)
    context = {
        'numbers': numbers,
    }
    return render(request, 'lotto.html', context)
```

```django
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>제 OOO회 로또 번호 추천</h1>
  <p> SSAFY님께서 선택하신 로또 번호는 {{ numbers }}입니다.</p>
</body>
</html>
```
