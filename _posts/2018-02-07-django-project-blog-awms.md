---
layout: post
title: 'Django Project - Blog/AWMS'
date: 2018-02-07
#cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: django
---

# 1. Create a django project
## 1.1 Command [>>Deatils](http://davidkor.logdown.com/posts/5303036)

$ ```mkdir 180205```

$ ```cd 180205```

$ ```python3 -m venv myvenv```

$ ```source myvenv/bin/activate```

$ ```pip install django```

$ ```django-admin.py startproject myproject```

$ ```cd myproject```

$ ```python manage.py startapp webapp```

$ ```python manage.py makemigrations```

$ ```python manage.py migrate```

$ ```python manage.py runserver```

> myproject/settings.py, change python to python3, told in the vedeo.
> ```#!/usr/bin/env python3```
> the project will work well even you don't change, just as what we did in the last project.

## 1.2. settings.py

### 1.2.1 INSTALLED APPS

```python
INSTALLED_APPS = [
	......
  'webapp',
  ]
```
### 1.2.2 TEMPLATES

```python
TEMPLATES = [
	......
  'DIRS': [os.path.join(BASE_DIR, 'templates').replace('\\', '/')],
  ......
```

### 1.2.3 STATICFILES_DIRS

```python
	STATICFILES_DIRS = (os.path.join(BASE_DIR, "static"),)
```

## 1.3. static, templates

> Some will say that static and templates folder should created in the same level the myvenv and myproject,
> Some also says, it doesn't matter even it is created under the webapp.
>>[Link to](http://davidkor.logdown.com/posts/5303036):#5 Templates, Static
>> It's said that the folder templates and static should be created at the same folder level as [startapp:django_web], in another word, the 2 folders should be created under [startproject:my_project]. Actually, as last ariticle showed that even you don't follow the rule, it still works.

> Fuck them off !!!!! ok？

Go to the folder 'webapp' and create folder 'static' and 'templates' by Finder.

Copy the css and js...files to 'static', copy html files to 'templates'

You can use the datalab web that we made through [HTML Page by SemanticUI](http://davidkor.logdown.com/posts/5395351-html-page-by-semanticui) and  [Show Highcharts.js in HTML with Dropdown Menu](http://davidkor.logdown.com/posts/5404393).

```python
{% raw %}
{% load static %} #insert at the first line of the html file
href="{% static 'css/semantic.css' %}" #change css path by template language
{% static 'images/star_banner.jpg' %} #change image path by template language
{% endraw %}
```

## 1.4. views.py

```python
def default(request):
	return render(request, 'default.html')
```

## 1.5. urls.py

```python
from webapp.views import default
urlpatterns = [
	url(r'^default', default),
]
```

```yay! by now, the basic completed``` Details: [# 0.Preparation](http://davidkor.logdown.com/posts/5408258-django-template-inheritance)

> Let's move on.
Create super user
->
models.py - create Database
->
admin.py - add to Admin page
->
models.py -  'context' to get db context for htmlpage
->
default.html - rewrite by django template language

# 2. Super user

$ ```python manage.py createsuperuser```

```
Username (leave blank to use 'YOUR_NAME'):
Email address: your_name@yourmail.com
Password:
Password (again):
Superuser created successfully.
```

# 3. Create Database in models.py

```python
class Caselog(models.Model):
    caseid = models.CharField(null=True, blank=True,max_length=200)
    orgnization = models.CharField(null=True, blank=True,max_length=200)
    problem = models.CharField(null=True, blank=True,max_length=200)
    solution = models.TextField(null=True, blank=True)
    handleby = models.CharField(max_length=100, default='',
                                choices=((u'Andy',u'Andy'),(u'David',u'David')))
    date = models.DateField(null=True)
    def __str__(self):
        return self.caseid
```

$ ```python manage.py makemigrations```
$ ```python manage.py migrate```

# 4.admin.py
Add data that you want to manage in admin console.
```python
from webapp.models import Caselog
admin.site.register(Caselog)
```

# 5. models.py

```python
from webapp.models import Caselog
def default(request):
    context = {
        'Caselog': Caselog.objects.all()
    }
    return render(request, 'default.html', context)
```
# 6. default.html

```html
{% for item in Caselog %}
<div class="item">
<div class="content">
<div class="header">
{{ item.caseid }}
</div>
<div class="description">
{{ item.orgnization }}
</div>
<div class="extra">
<div class="ui label">
{{ item.handleby }}
</div>
<div class="ui label">
{{ item.date }}
</div>
</div>
</div>
</div>
{% endfor %}
```

>  [Issue with highlighting load tags for Django code #4567](https://github.com/jekyll/jekyll/issues/4567) 
