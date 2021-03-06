---
layout: post
title: 'Django Template Language'
date: 2018-01-25
tags: django
---
장고 템플릿 언어
> All the steps should be finished before start.
> Refer to: [ [Ultimate] Django Web created by Command](http://davidkor.logdown.com/posts/5303036)

# 1. render(request, x.html, context)

## 1.1 views.py

```Python
def index(request):
    context = {
        'title':'caseid',
        'content':'company',
    }
    return render(request, 'index.html', context)
```
## 1.2 index.html
```html
<div class="header">
	{{ title }}
</div>
<div class="description">
  <p>
 	 {{ content }}
  </p>
</div>
```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/sSXY27TQCOvpoz34T0us_1.png)

# 2. Connect to Mongodb

> Django has integrated with sqlite3, however, for easy data sorting, we need to import mongoengine.

## 2.1 settings.py
Django will run settings.py first to check all the configrations.

```Python
from mongoengine import connect
connect('awms', host='127.0.0.1', port=27017)
```

## 2.2 models.py

```Python
from mongoengine import *
from mongoengine import connect
connect('awms', host='127.0.0.1', port=27017)

class LogInfo(Document):
    caseid = StringField()
    orgnization = StringField()

    meta = {'collection':'loginfo_backup'}

for i in LogInfo.objects[:1]:
    print(i.caseid)
```


-----
# PROBLEM-1:mongoengine module package need to be installed
Pycharm - Files - Default Settings - Project Interpreter - + - mongoengine - install

obviously, it is the easy and truly correct way to install mongoengine package, BUT when run models.py, error occurred 'ModuleNotFoundError: No module named 'mongoengine''
# ANSWER
Go to the project root path use command to install mongoengine package.
$ ```~/PycharmProjects/180125/my_project ⮀ pip install mongoengine```

[Reference](https://www.douban.com/note/513251201/)
> Donno why ㅠㅜ

-----
# PROBLEM-2:mongoengine.errors.FieldDoesNotExist
the fields "{'note',...}" do not exist on the document "LogInfo"
# ANSWER
All the columns in db should be defined here.

[Reference](http://blog.csdn.net/depers15/article/details/52234311)

-----

## 2.2' models.py

```Python
from mongoengine import *
from mongoengine import connect
connect('awms', host='127.0.0.1', port=27017)

class LogInfo(Document):
    caseid = StringField()
    problem = StringField()
    custtpye = StringField()
    orgnization = StringField()
    handle_by = StringField()
    city = StringField()
    no = StringField()
    date = StringField()
    attachment = StringField()
    solution = StringField()
    source = StringField()
    resolved = StringField()
    note = StringField()

    meta ={'collection':'loginfo_backup'}

for i in LogInfo.objects[:1]:
    print(i.caseid, i.date)
```

If the database already exits ```mata={}```, otherwise no need.
![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/9qUYc07QzCkSSBhR7OWQ_1.png)

## 2.3 views.py

```Python
from django.shortcuts import render
from django_web.models import LogInfo

def index(request):
    log_info = LogInfo.objects[:1]
    context = {
        'title': log_info[0].caseid,
        'content':log_info[0].orgnization,
    }
    return render(request, 'index.html', context)
```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/1xhM2XGQvK7upXpK1ubH_2.png)


![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/itHNfJZDR4GgEKxeOZXG_3.png)

#2.4 Paginator
Firstly, get to know how paginator works.

views.py - console
```Python
$ from django.core.paginator import Paginator
$ iter = 'qwertyuiop'
$ paginator = Paginator(iter,3)
$ paginator.page(1)
<Page 1 of 4>
$ page1 = paginator.page(1)
$ page1.object_list
'qwe'
$ page1.has_next()
True
$ page1.number
1
$ page1.paginator.num_pages
4
```
Let's jump into codes.

* views.py

```Python
from django.shortcuts import render
from django_web.models import LogInfo
from django.core.paginator import Paginator

def index(request):
    limit = 10
    log_info = LogInfo.objects[:10]
    paginator = Paginator(log_info, limit)
    page = request.GET.get('page', 1)
    loaded = paginator.page(page)
    context = {
        'LogInfo':loaded
    }
    return render(request, 'index.html', context)
```

![4.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/V16G0fPYTgWvvjvwHbYf_4.png)


* index.html

```html
<div class="ui divided items">

  {% for item in LogInfo %}
    <div class="item">
      <div class="content">
        <div class="header">    {{ item.caseid }}    </div>
        <div class="description">
        <p>    {{ item.orgnization }}    </p>
        </div>
        <div class="extra">
          <div class="ui label">    {{ item.date }}    </div>
          <div class="ui label">    {{ item.handle_by }}    </div>
        </div>
      </div>
    </div>
  {% endfor %}

</div>
```

![5.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/Vi4DKmXT7alzAvSumqHb_5.png)

![6.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/2mCTlqTXTQdFy6vo1Y0Q_6.png)


* Activate the paginator

A sample of how the paginator looks like.
```html
<a> 《 Previous </a>
<span> 1 of 4 </span>
<a> Next 》 </a>
```
Then use template language to activate it.
```html
<div class="ui small pagination menu">
    {% if LogInfo.has_previous %}
    	<a href="?page={{ LogInfo.previous_page_number}}" class="active item"> < </a>
    {% endif %}

    <div class="disabled item">
   		{{ LogInfo.number }} of {{ LogInfo.paginator.num_pages }}
    </div>

    {% if LogInfo.has_next %}
   		<a href="?page={{ LogInfo.next_page_number }}" class="item"> > </a>
    {% endif %}
</div>
```
![9.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/Cb31UvFSeefNg5c3Dqtu_9.png)

> What does ```page = request.GET.get('page', 1)```really mean?

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5303690/iYieTpGbSNicfplFsQCg_1.png)

> Due to the video, ```[:10]```should be removed in  ```log_info = LogInfo.objects[:10]```
> cuz it means only find 10 items
> BUT, even it wasn't deleted, the web page could get data well
>Donno why????
