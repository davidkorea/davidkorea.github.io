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
> [Link to](http://davidkor.logdown.com/posts/5303036):#5 Templates, Static

Go to the folder 'webapp' and create folder 'static' and 'templates' by Finder.

You can use the datalab web that we made through [HTML Page by SemanticUI](http://davidkor.logdown.com/posts/5395351-html-page-by-semanticui) and  [Show Highcharts.js in HTML with Dropdown Menu](http://davidkor.logdown.com/posts/5404393).

```python
{% load static %} #insert at the first line of the html file
href="{% static 'css/semantic.css' %}" #change css path by template language
{% static 'images/star_banner.jpg' %} #change image path by template language
```
