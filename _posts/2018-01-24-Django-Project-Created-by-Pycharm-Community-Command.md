---
layout: post
title: 'Django Project Created by Pycharm Community + Command'
date: 2018-01-24
tags: django
---
> This is an updated approach to create Django project by Pycharm community version which is much easier and more reasonable than the former one: [ Django Web created by Pycharm+Command](http://davidkor.logdown.com/posts/4716406)


# 1.Pycharm - new project

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/jRV2IJYmSQqJb9zZEICB_1.png)

we don't use pycharm to create a virtual environment.

# 2.Command
$```cd PycharmProjects```

$```cd 180124_3```

$```python3 -m venv my_venv```

$```cd my_venv```

$```source bin/activate```

$```pip install django```

$```django-admin.py startproject my_project```

$```cd my_project```

$```python manage.py migrate```

$```python manage.py runserver```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/53qZkHTJ2SBSr8BvquAg_2.png)

# 3.Run 127.0.0.1:8000

![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/PvTmLclmRU2j92agJ1KR_3.png)

# 4.python manage.py startapp django_web
$```python manage.py startapp django_web```

# 5.Add django_web to installed app

my_project/settings.py

![4.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/JdT46TmIQPWt6dP2Jtsi_4.png)

# 6.mkdir templates under django_web

mkdir templates under django_web
copy index.html to templates folder

# 7.views.py

django_web/views.py

```Python
def index(request):
    return render(request, 'index.html')
```

![5.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/LoRWZnGeTSit0iaefvQA_5.png)

# 8.urls.py
 my_project/urls.py

```Python
{% raw %}
from django_web.views import index
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/', index),
]
{% endraw %}
```

![6.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/C3eO5JciS5GDhuzIxuFK_6.png)

# 9.Get access to 127.0.0.1:8000/index

![7.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/Z08OaWjpTt2qK3CYwr34_7.png)

it can run normally except css. then we will continue to add css to index.html

# 10."static"
$```{% raw %}mkdir static{% endraw %}```,  copy 'css' and 'images'folder to "static"

go to templates/index.html, put the following code on the 1st line of this html file
```Python
{% raw %}
  {% load static %}
{% endraw %}
```
change all the css styles and image paths by using ```{% raw %}{% static 'oringin path' %}{% endraw %}```,sample as follows

```
{% raw %}
<link rel="stylesheet" type="text/css" href="{% static 'css/semantic.css' %}">
<img src="{% static 'images/0001.jpg' %}" width="100" height="91">
{% endraw %}
```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/L90aIjAHSrGWwzv7U2YX_1.png)

but, the "static files" may not be found by codes, so we need to set the absolute path in settings.py
```Python
{% raw %}
STATICFILES_DIRS = (os.path.join(BASE_DIR, "static"),)
{% endraw %}
```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/zSW8jhhR0mNfUcmOJrYR_2.png)

Done, html can run with css and give up a wonderful page!!

![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/5298745/zrK26yiTKiYOeuLKod5A_3.png)
