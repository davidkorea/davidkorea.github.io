---
layout: post
title: 'Django Project - Blog/AWMS'
date: 2018-02-07
author: Jekyll
#cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: django
---

# 1. Create a project

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
