---
layout: post
title: 'Django Web created by Pycharm & Command'
date: 2018-01-03
tags: pycharm django
---

# 1.Pycharm - Community Edition

![1.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4716406/OX2sU3HlQkuGC86nodOk_1.jpg)

# 2.Command
 iTerm

![2.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4716406/xhJsDAEITsGm9xdeKXnY_2.jpg)
```Python
  cd 180103
  mkdir david
  cd david
  python3 -m venv my_venv
  cd my_venv
  source bin/activate
  pip install django
  django-admin.py startproject my_project
  cd my_project
  python manage.py migrate
  python manage.py runserver
```
![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4716406/rCrpZYTs2AW9oaDVBX7Q_1.png)


# 3. Pycharm Terminal
 이어서 계속 iTerm에서 코딩해도 됩니다만 Pycharm Terminal을 이용하려면 추가로 다시 한번 venv를 껴놔야 됩니다.

![3.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4716406/sFVmeO8vRDWQRmbbZQKZ_3.jpg)
```Python
 cd david
 cd my_venv
 source bin/activate
 cd my_project
 python manage.py createsuperuser
```


![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4716406/J6Z7hBNCRqC190lDzxjp_2.png)

#### reference
[create django web by command](https://djangogirlstaipei.gitbooks.io/django-girls-taipei-tutorial/content/django/installation.html)
