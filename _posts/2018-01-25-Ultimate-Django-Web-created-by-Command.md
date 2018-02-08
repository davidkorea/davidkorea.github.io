---
layout: post
title: '[Ultimate] Django Web created by Command'
date: 2018-01-25
tags: django
---
> An updated version of the [last aritical](http://davidkor.logdown.com/posts/5298745).
Due to the [last ariticle](http://davidkor.logdown.com/posts/5298745) is still 번거로워...lets use only command line to create a Django project.let's start ㄱ

# 1. Command

$ ```mkdir 180125```

$ ```cd 180125```

$ ```python3 -m venv my_venv```

$ ```source my_venv/bin/activate```

$ ```pip install django```

$ ```django-admin.py startproject my_project```

$ ```cd my_project```

$ ```python manage.py runserver```


![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/F650caD7To2xbaYt9MSS_1.png)

# 2. Run 127.0.0.1:8000

# 3. Open by Pycharm and create app

There is no need to run venv again in pycharm terminal, cuz it has been started automatically.
go to my_project and startapp directly.

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/Oh3lEWGkSDOnXRt9augG_2.png)

-----

Refer to : [djangogirls](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/installation.html)

-----

# 4. add app to my_project/settings.py

my_project/settings.py
```Python
INSTALLED-APPS = [
	...
	'django_web'
]
```

# 5. Templates, Static

For make the process visible, i would like to create the relevant folders in Finder.

It's said that the folder templates and static should be created at the same folder level as [startapp:django_web], in another word, the 2 folders should be created under [startproject:my_project]

Actually, as last ariticle showed that even you don't follow the rule, it still works.

Whatever, just do as the rule, for coding more reasonalbe.


![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/pK1fRXwTkyrG3HUu5l2V_3.png)

Then, copy index.html to templates and copy css,js,font,images to static


![4.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/XkT1K3tGSXusojVHSbfU_4.png)

# 6. Rewrite index.html by template languague

Add ```STATICFILES_DIRS = (os.path.join(BASE_DIR, "static"),)``` to settings.py

![5.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/TH2B7jvmSieuBQmtttPv_5.png)

# 7. django_web/views.py

django_web/views.py
```Python
def index(request):
    return render(request, 'index.html')
```

![7.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/1AhgMFTLWCK0qTwNxwJw_7.png)

# 8. my_project/urls.py

my_project/urls.py

```Python
from django_web.views import index

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/', index),
]
```

![8.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/9bubl9JRS6u2PrFMEote_8.png)

-----

# ========== ERROR ==========


![6.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/W5DgRVTRPFB2zKwMoVAg_6.png)

# ========== ANSWER ==========

add ```[os.path.join(BASE_DIR, 'templates')]```to TEMPLATES 'DIRS' in settings.py
[Reference](https://www.zhihu.com/question/61051824)
![9.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/6jh9BOOVTFyOavIL0Kjf_9.png)

![10.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303036/QjhV0HSXQSKeTpBsqeGt_10.png)

> Haven't figured out what's the problem for the TEMPLATES DIR. BUT, it works without this code in [last aritical](http://davidkor.logdown.com/posts/5298745).

> The ONLY different thing is that i created templates folder by mkdir in pycharm terminal in the last article.

> I found that the video codes also has  ```[os.path.join(BASE_DIR, 'templates')]``` in the settings.py file. cuz the templates folder was created in Finder. BUT it wasn't taughted in the video about this piece of code.

-----
