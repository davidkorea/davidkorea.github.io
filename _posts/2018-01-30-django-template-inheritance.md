---
layout: post
title: 'Django Template Inheritance'
date: 2018-01-30
tags: django
---
> ```Command + K``` 서버에 연결, 윈도우 쉐어 플더 연결
> At the very beginning, we need to import [what we have done last time](http://davidkor.logdown.com/posts/5404393) into [django project 20180125,](http://davidkor.logdown.com/posts/5303690) including, html webpage and charts js files.
> More details in 0.Preparation

# 0. Preparation

## 0.1 Import relevant files

template <- default.html
statics <- js/exporting.js, js/highcharts-more.js, js/highcharts.js

## 0.2 Django project urls.py, views.py

To open default.html in django web, view functions and url should be defined

* views.py

```Python
def default(request):
	return render(request, 'default.html')               X
  <!-- return render(request, 'default_data.html') -->   O
```

* urls.py

```Python
from django_web.views import index
from django_web.views import default
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/', index),
    url(r'^default/', default),
]
```

* Rewrite dafault.html by Template Language

```Python
{% raw %}
{% load static %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>DataLab</title>
    <link rel="stylesheet" href="{% static 'css/semantic.css' %}">
    <script src="{% static 'js/jquery.min.js' %}"></script>
    <script src="{% static 'js/semantic.js' %}"></script>
    <script src="{% static 'js/exporting.js' %}"></script>
    <script src="{% static 'js/highcharts-more.js' %}"></script>
    <script src="{% static 'js/highcharts.js' %}"></script>
  </head>
{% endraw %}
```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5408258/nTkaOlVVTqyTppsGG2S9_1.png)

-----

# 1. Template Inheritance

As the picture shows, things in the red line box should be removed except the main framework including sidebar and ui menu.


![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5408258/qO3jKqUkSAqlW7oO4ica_2.png)

## 1.1 Move to sub template （LogInfo data field）

* create sub template 'default_data.html'
* copy the 'ui equal width grid' from default.html to default_data.html
* relate main template with sub template

main template: default.html
```Python
{% raw %}
{% block grid %} {% endblock %}
{% endraw %}
```

> ```{% raw %}{% block grid %}{% endraw %}```, the ```grid```can be customized.

sub template: default_data.html
```Python
{% raw %}
{% extends 'default.html' %}

{% block grid %}
<div class="ui equal width grid" style=...>
{% endblock %}
{% endraw %}
```

-----

# ========== FAILED ==========

Template inheritance failed!!!!! The content moved to sub template cannot be shown...

# ========== ANSWER ==========
The correct views.py should be like this: ```def default(request):```
wrong: ```return render(request, 'default.html')```
correct: ```return render(request, 'default_data.html')```

> Cuz the sub template has already have the full page contents.
> Thats's why views.py should render the sub template.
> ```{% raw %}{% extends 'default.html '%}{% endraw %}``` means abtain all the main template's contents,
> and the empty block in the main template will be filled with
> ```{% raw %}{% block grid %}......{% endblock %}{% endraw %}```, which belongs to the sub template.

----

## 1.2 Charts

1.create default_charts.html

2.fill default_charts.html with ```{% raw %}{% block grid %}{% endraw %}``` and charts js scripts

3.default_charts.html

```Python
{% raw %}
{% extends 'default.html' %}
{% block grid %}
	<div>... ...</div>
{% endblock %}

{% block chartsjs %}
	<script>... ...</script>
	<script>... ...</script>
{% endblock %}
{% endraw %}
```

4.views.py

```Python
def charts(request):
    return render(request, 'default_charts.html')
```

5.urls.py

```Python
from django_web.views import index, default, charts
urlpatterns = [
	... ...
	url(r'^charts/', charts)
]
```

6.views.py Again

> Reference:  [Sort data by jupyter 3](http://davidkor.logdown.com/posts/4773093)

```Python
# =====Data Generator=====
def topx_data_gen(date1, date2, member, limit):
    pipeline = [
        {'$match': {'$and': [{'date': {'$gte': date1, '$lte': date2}}, {'handle_by': member}]}},
        {'$group': {'_id': '$city', 'counts': {'$sum': 1}}},
        {'$sort': {'counts': -1}},
        {'$limit': limit}
    ]

    for i in LogInfo._get_collection().aggregate(pipeline):
        data = {
            'name': i['_id'],
            'data': [i['counts']],
            'type': 'column'
        }
        yield data

series_andy = [i for i in topx_data_gen('2017/01/01','2017/12/31','Andy Tsao',10) ]
series_david = [i for i in topx_data_gen('2017/01/01','2017/12/31','David Liu',10) ]
#============end===============
def charts(request):
    context = {
        'chart_andy': series_andy,
        'chart_david': series_david,
    }
    return render(request, 'default_charts.html', context)
```


7.dafault_charts.html Again
```Python
{% raw %}
{% extends 'default.html' %}

{% block grid %}
      <div class="ui equal width grid" style="width:75%;margin:5px 5px 5px 5px;">
        <div class="row">
          <div class="column">
            <div class="ui simple dropdown item">
              Menber
              <i class="dropdown icon"></i>
              <div class="menu">
                <div class="item" id="andy">Andy</div>
                <div class="item" id="david">David</div>
              </div>
            </div>

            <div class="ui container segment">
              <div class="container" id="chart1">
                Top 10 Cities in 2017 by member
              </div>
            </div>
          </div>
        </div>
      </div>
{% endblock %}

{% block chartsjs %}
<script>
      $('#andy').click(function () {
        $('#chart1').highcharts({
          credits:{
              enabled:false
          },
          title: {
              text: 'Top 10 Cities in 2017 by Andy'
          },

          series: {{ chart_andy|safe }}
      });
  });
</script>

<script>
      $('#david').click(function () {
        $('#chart1').highcharts({
          credits:{
              enabled:false
          },
          title: {
              text: 'Top 10 Cities in 2017 by David'
          },

          series: {{ chart_david|safe }}
      });
  });
</script>
{% endblock %}
{% endraw %}
```
> INPORTANT THING!!!
> ```{{ chart_adny|safe }}```, ```|safe``` is a must. or the chart will not be shown due to the unicode error.

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5408258/xkX51UmZRpumG8eXeZbC_1.png)


8.NOT Necessery, BUt can test in models.py

```Python
pipeline = [
    {'$match': {'$and': [{'date':{'$gte':'2017/01/01','$lte':'2018/01/09'}},{'handle_by':'Andy Tsao'}]}},
    {'$group': {'_id':'$city', 'counts': {'$sum': 1}}},
    {'$sort':{'counts':-1}},
    {'$limit':10}
]
for i in LogInfo._get_collection().aggregate(pipeline):
    print(i)
```

# 2. Rewrite sidebar

Set the correct link on the side mwnu.

main template: default.html
```html
    <div class="ui thin visible left sidebar inverted vertical menu" id="sidebar">
      <div class="item"><a href="/default">Index</a></div>
      <div class="item"><a href="/charts">Charts</a></div>
    </div>
```

> ```href="..."``` should use the path set in urls.py
> NOT the html file name
