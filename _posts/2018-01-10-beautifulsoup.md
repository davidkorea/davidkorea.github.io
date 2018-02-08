---
layout: post
title: 'BeautifulSoup'
date: 2018-01-10 06:58
comments: true
categories: [beautifulsoup]
---
# 0-1.TODAY'S FU*KIN CASE 1
 if you create a python file named ```python.py```, then all the python files will run with an error like this,  ``` ModuleNotFoundError: No module named 'html.entities'; 'html' is not a package``` 

errer as below
```
 Traceback (most recent call last):
  File "/Users/osx/PycharmProjects/awmshtml/variable.py", line 1, in <module>
    from bs4 import BeautifulSoup
  File "/usr/local/lib/python3.6/site-packages/bs4/__init__.py", line 35, in <module>
    from .builder import builder_registry, ParserRejectedMarkup
  File "/usr/local/lib/python3.6/site-packages/bs4/builder/__init__.py", line 7, in <module>
    from bs4.element import (
  File "/usr/local/lib/python3.6/site-packages/bs4/element.py", line 10, in <module>
    from bs4.dammit import EntitySubstitution
  File "/usr/local/lib/python3.6/site-packages/bs4/dammit.py", line 14, in <module>
    from html.entities import codepoint2name
ModuleNotFoundError: No module named 'html.entities'; 'html' is not a package
```
> Answer:
> Never create a python file named ```html.py``` in your project
> refer to :[http://blog.csdn.net/gllg1314/article/details/54345773](http://blog.csdn.net/gllg1314/article/details/54345773)

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4732656/T6oJ4afwRniE7S0tOIxV_1.png)

# 0-2.TODAY'S FU*KIN CASE 2

error occurred everytime when the local html file has too many codes```UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 in position 1652: ordinal not in range(128)```

source as below
```python
from bs4 import BeautifulSoup

path = '/Users/osx/Desktop/landing_page.html'
with open(path, 'r') as wb_data:
    soup = BeautifulSoup(wb_data, 'lxml')
    print(soup)
```
> Answer:
> ```with open(path, 'r', encoding='utf-8') as wb_data: ```
> refer to : [https://stackoverflow.com/questions/41512427/codecs-ascii-decodeinput-self-errors0-unicodedecodeerror-ascii-codec-can](https://stackoverflow.com/questions/41512427/codecs-ascii-decodeinput-self-errors0-unicodedecodeerror-ascii-codec-can)

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4732656/PGLzho3Q52yIQXn0t0NA_1.png)

---


# 1.html as a variable

```python
from bs4 import BeautifulSoup

html = '''
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
'''

soup = BeautifulSoup(html, 'lxml')
print(soup)
```
![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4732656/3CwKV0vdQOedIhtPmewT_2.png)

# 2. local html page

```python
from bs4 import BeautifulSoup

path = '/Users/osx/Desktop/landing_page.html'
with open(path, 'r', encoding='utf-8') as wb_data:
    soup = BeautifulSoup(wb_data, 'lxml')
    print(soup)
```

# 3.web page

```python
from bs4 import BeautifulSoup
import requests

url = 'http://davidkor.logdown.com/'
wb_data = requests.get(url)
soup = BeautifulSoup(wb_data.text, 'lxml')
print(soup)
```