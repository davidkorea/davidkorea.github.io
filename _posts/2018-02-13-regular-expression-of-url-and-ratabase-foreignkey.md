---
layout: post
title: "Regular Expression of Url and Batabase ForeignKey"
date: 2018-02-13
tags: django regularexpression foreignkey
---

# 1. Url

## 1.1 Patterns of URL of Django

- ```(r'^detail'```

  - detail/abc.def
  - detail123456
  - detail_%123a

- ```(r'^detail$'```

  - abc/def/detail
  - 123456detail
  - %12avdetail

- ^detail$

  - www.blog.com/detail

- Regular Expression 정규표현식

  - /detail/(\d){3}, 3 numbers

  - /detail/(\d+), no limited numbers

## 1.2 Asign each post a url

> [Github commit](https://github.com/davidkorea/180205/commit/707056f6da41fa96ec5f8c5cd89951e6ffab0a54)

### 1.2.1 urls.py

```Python
url(r'^detail/(?P<page_num>\d+)$', detail, name='detail'),
```

### 1.2.2 views.py

```Python
+ def detail(request, page_num):
     if request.method == 'GET':
         form = CommentForm
     if request.method == 'POST':
         form = CommentForm(request.POST)
         if form.is_valid():
             name = form.cleaned_data['name']
             comment = form.cleaned_data['comment']
             c = Comment(name = name, comment = comment)
             c.save()
+            return redirect(to='detail', page_num=page_num)
     context = {}
     comment_list = Comment.objects.all()
+    caselog = Caselog.objects.get(id=page_num)
+    context['caselog'] = caselog
     context['comment_list'] = comment_list
     context['form'] = form
     return render(request, 'log_detail.html', context)
```
> 'return redirect(to='detail', page_num=page_num)', error will occur if no page_num when submit a form.

### 1.2.3 templates

- log_detail.html

```HTML
{% raw %}
<div class="ui  segment padded container" >
   <h1 class="ui header" style="font-family:'Oswald', sans-serif;font-size:40px">
      {{ caselog.problem }}
   </h1>
   <p>
      {{ caselog.solution }}
   </p>
</div>
{% endraw %}
```

- default.html

```HTML
{% raw %}
{% for item in Caselog %}
  <div class="item">
    <div class="content">
      <a href="{% url 'detail' item.id %}"> # detail/db.id=>'/detail/1'
        <div class="header">
          {{ item.caseid }}
        </div>
      </a>

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
{% endraw %}
```

# 2. Each post shows its own comments

Relate comments with its post.

> just like in Ruby, databases have 'belongs_to & has_many' to bind together.
> **Caselog** has many **Comment**, **Comment** belongs to **Caselog**.

## 2.1 models.py

```Python
class Comment(models.Model):
+  belong_to = models.ForeignKey(to=Caselog, related_name='under_comments', null=True, blank=True)
```
- 'to': relate to **Caselog** database.
- 'related_name': same as insert a column 'under_comments' in **Caselog**. BUT you can use **Caselog.under_comments** even you don't really need to insert this column in **Caselog**.
- **Comment.belong_to** == **Caselog.under_comments**

Then update database by using makemigrations and migrate.

FKin ERROR!```TypeError: __init__() missing 1 required positional argument: 'on_delete'```.
ANSWER
```Python
class Comment(models.Model):
+  belong_to = models.ForeignKey(to=Caselog, related_name='under_comments', null=True, blank=True, on_delete=models.CASCADE)
```
[Reference](https://stackoverflow.com/questions/44026548/getting-typeerror-init-missing-1-required-positional-argument-on-delete)

## 2.2 views.py

### 2.2.1 Caselog.under_comments

> [Github commit](https://github.com/davidkorea/180205/commit/707056f6da41fa96ec5f8c5cd89951e6ffab0a54)

- Disable comment_list, only use caselog.under_comments.all to show comments
```Python
def detail(request, page_num):
  if request.method == 'GET':
      form = CommentForm
  if request.method == 'POST':
      form = CommentForm(request.POST)
      if form.is_valid():
          name = form.cleaned_data['name']
          comment = form.cleaned_data['comment']
          a = Caselog.objects.get(id=page_num) # we need to save data of the new created column in db
          c = Comment(name = name, comment = comment, belong_to=a)
          c.save()
          return redirect(to='detail', page_num=page_num)
  context = {}
  # comment_list = Comment.objects.all()
  caselog = Caselog.objects.get(id=page_num) # ONLY use caselog.under_comments
  context['caselog'] = caselog
  # context['comment_list'] = comment_list
  context['form'] = form
return render(request, 'log_detail.html', context)
```

- Go to admin console set the value of 'belong_to', then the comments could be bind with the only post. Reference: [let admin console manage Comment db](https://github.com/davidkorea/180205/commit/1ce07a25e75e56d4ad7bae5a3978a10ab19aab7e)

  > Why we need to select 'belong_to' manually?
  > cuz the comments are the one we submit before creating 'belong_to' column.
  > New comments could be set automatically by now through the foloowings
  > ```a = Caselog.objects.get(id=page_num)```and ``` c = Comment(name = name, comment = comment, belong_to=a)``` and ```c.save()```.

![1](https://raw.githubusercontent.com/davidkorea/blogdata/master/20180213/image/1.png)


### 2.2.2 Comment.belong_to / NEW functon 'Best Comment'

- Add a BooleanField column to **Comment** db in models.py.

```Python
class Comment(models.Model):
  best_comment = models.BooleanField(default=False)
```
  migrate!

- Check a best_comment in Admin console.

- views.py

```Python
def detail(request, page_num):
  if request.method == 'GET':
      form = CommentForm
  if request.method == 'POST':
      form = CommentForm(request.POST)
      if form.is_valid():
          name = form.cleaned_data['name']
          comment = form.cleaned_data['comment']
          a = Caselog.objects.get(id=page_num) # we need to save data of the new created column in db
          c = Comment(name = name, comment = comment, belong_to=a)
          c.save()
          return redirect(to='detail', page_num=page_num) # for redirect through page_num
  context = {}
  a = Caselog.objects.get(id=page_num)
  best_comment = Comment.objects.filter(best_comment=True, belong_to=a)
  if best_comment:
      context['best_comment'] = best_comment[0]
  ... ...
```
  * 'belong_to' needs a db class instance.
  * 'Caselog.objects.get(id=page_num)' means get a **Caselog** through page_num.
  * 'Comment.objects.filter()', filter will returna a list.

- template

```HTML
{% raw %}
{% if best_comment %}
    <div class="ui mini red left ribbon label">
        <i class="icon fire"></i>
        BEST
    </div>
    <div class="best comment">
        <div class="avatar">
            <img src="http://semantic-ui.com/images/avatar/small/matt.jpg"  alt="" />
        </div>
        <div class="content">
            <a href="#" class="author">{{ best_comment.name }}</a>
            <div class="metadata">
                <div class="date">2 days ago</div>
            </div>
            <p class="text" style="font-family: 'Raleway', sans-serif;">
                {{ best_comment.comment }}
            </p>
        </div>
    </div>
    <div class="ui divider"></div>
{% endif %}

{% for comment in article.under_comments.all %}
  ......
{% endfor %}
{% endraw %}
```

  > Somethings strang!!! Best and normal comments shows duplicated.
