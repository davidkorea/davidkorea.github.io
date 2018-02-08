---
layout: post
title: 'Crawl data from 58.com 2nd-hand products'
date: 2018-01-06
tags: python mongodb
---
# 0.Workflow

> 1.Create a project with pycharm community version
> 2.Get channel list
> 3.Get each channel's product links
> 4.Get each product's detailed info

# 1.Create a project

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/35eHehBYSomLTHl469WX_2.png)


# 2. Get channel list

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/C9ENwRShRFWHiLUquErF_1.png)

## 2.1 check css select path
```python
from bs4 import BeautifulSoup
import requests

start_url ='http://jn.58.com/sale.shtml'

def get_channel_urls(url):
    wb_data = requests.get(url)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    links = soup.select('ul.ym-submnu > li > b > a')
    print(links)

get_channel_urls(start_url)

```
![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/l6NU6wmuS4W0pNkRIbDR_3.png)


## 2.2 Crawl channel links and save it as a list

```python
from bs4 import BeautifulSoup
import requests

start_url ='http://jn.58.com/sale.shtml'
url_host = 'http://jn.58.com'

def get_channel_urls(url):
    wb_data = requests.get(url)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    links = soup.select('ul.ym-submnu > li > b > a')
    for link in links:
        page_link = url_host + link.get('href')
        print(page_link)

get_channel_urls(start_url)

```
![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/Wnggj5I6QcGJst1MB85E_1.png)


![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/c7X24GURmeaeYNOZDxg9_2.png)

# 3. Parse product page and get each product's link
    > parse
      vt.从语法上描述或分析（词句等）
      [例句]Parse files: files in this filter are parsed for autocomplete and other designers.
      分析文件：针对自动完成和其他设计器来对该筛选器中的文件进行分析。


![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/RIoM2iujS96M9OTrt267_1.png)

## 3.1 check css select path

```python
from bs4 import BeautifulSoup
import requests
import time
import pymongo

# client = pymongo.MongoClient('localhost', 27017)
# ceshi_58 = client['ceshi_58']
# product_url_list = ceshi_58['product_url_list']

#spider1

def get_product_links_from(channel, pages, who_sells=0):
    # http://jn.58.com/shouji/0/pn2/?islocal=1
    link_view = '{}{}/pn{}/?islocal=1'.format(channel,str(who_sells), str(pages))
    wb_data = requests.get(link_view)
    time.sleep(1)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    product_link = soup.select('td.t > a.t')
    print(product_link)

get_product_links_from('http://jn.58.com/shouji/', 2)
```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/Riyyw2FGTra3DIpEucnW_1.png)

## 3.2 Get product's link
```pythons
from bs4 import BeautifulSoup
import requests
import time
import pymongo

# client = pymongo.MongoClient('localhost', 27017)
# ceshi_58 = client['ceshi_58']
# product_url_list = ceshi_58['product_url_list']

#spider1

def get_product_links_from(channel, pages, who_sells=0):
    # http://jn.58.com/shouji/0/pn2/?islocal=1
    link_view = '{}{}/pn{}/?islocal=1'.format(channel,str(who_sells), str(pages))
    wb_data = requests.get(link_view)
    time.sleep(1)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    product_link = soup.select('td.t > a.t')
    for link in product_link:
        item_link = link.get('href').split('?')[0]
        print(item_link)

get_product_links_from('http://jn.58.com/shouji/', 2)

```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/NmvkGkJoSYqDTRNUHog6_1.png)

## 3.3 Save products' links to Mongodb

Let's make some changes on the code to be running more efficiently.

### 3.3.1 we should let the code to know if there is no such page, just pass. eg. page200

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/qc0ngYVRT6es6uYdlc6w_1.png)

use soup.find() to check whether the page has ```<div class="noinfotishi">```
BeautifulSoup's find() may a little different from select()
we can just input tab div and class noinfotishi with , divided to find the css selector path,like find('div', 'noinfotishi')
if use select(), we need to use select('div.nothfotishi')

```python
soup = BeautifulSoup(wb_data.text, 'lxml')
    if soup.find('td','t'):
        print('info')
    else:
        print('noinfo')
```
or
```python
soup = BeautifulSoup(wb_data.text, 'lxml')
    if soup.find('div', 'noinfotishi'):
        pass
    else:
        for link in soup.select('td.t a.t'):
            item_link = link.get('href').split('?')[0]
            product_url_list.insert_one({'url':item_link})
```

### 3.3.2 we need to ignore the first several advs by its link that contains 'jump'
```python
        for link in soup.select('td.t a.t'):
            item_link = link.get('href').split('?')[0]
            if 'jump' in item_link.split('/'):
                pass
            else:
                product_url_list1.insert_one({'url':item_link})
```

### 3.3.3 Before run page_parsing.py, run mongodb service first.
```
mongod
```
### 3.3.4  page_parsing.py
```python
from bs4 import BeautifulSoup
import requests
import time
import pymongo

client = pymongo.MongoClient('localhost', 27017)
ceshi_58 = client['ceshi_58']
product_url_list = ceshi_58['product_url_list']

#spider1

def get_product_links_from(channel, pages, who_sells=0):
    # http://jn.58.com/shouji/0/pn2/?islocal=1
    link_view = '{}{}/pn{}/?islocal=1'.format(channel,str(who_sells), str(pages))
    wb_data = requests.get(link_view)
    time.sleep(1)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    if soup.find('div', 'noinfotishi'):
        pass
    else:
        for link in soup.select('td.t a.t'):
            item_link = link.get('href').split('?')[0]
            if 'jump' in item_link.split('/'):
                pass
            else:
                product_url_list1.insert_one({'url':item_link})

get_product_links_from('http://jn.58.com/shouji/', 2)

```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/otaXAENaRFwdz0m07T0X_1.png)

# 4 Save item info to Mongodb

## 4.1 if the item are sould out, ignore it cuz no info on that page
eg: http://zhuanzhuan.58.com/detail/938627420277926918z.shtml

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/db6MOEIzSXa1NNYGxvk3_1.png)


## 4.2 get info in the tag
1. ```link = soup.select('ul.ym-submnu > li > b > a').get('href') ```
2. ```price = soup.select('span.price_now > i')[0].text```
3. ```area = list(soup.select('div.palce_li > span > i')[0].stripped_strings)```

## 4.3 if the tag is in this page then get it otherwise ignore
```python
 area = list(soup.select('div.palce_li > span > i')[0].stripped_strings) if soup.find_all('div', 'palce_li') else None
```


```python
def get_item_info(url):
    wb_data = requests.get(url)
    soup = BeautifulSoup(wb_data.text, 'lxml')
    no_longer_exist = soup.select('span.soldout_btn')
    if no_longer_exist:
        pass
    else:
        title = soup.select('h1')[0].text
        price = soup.select('span.price_now > i')[0].text
        area = list(soup.select('div.palce_li > span > i')[0].stripped_strings) if soup.find_all('div', 'palce_li') else None
        item_info.insert_one({'title':title, 'price':price, 'area':area})

get_item_info('http://zhuanzhuan.58.com/detail/948479863277731847z.shtml')
```
![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4724433/QDodt1QYTCqIHqZue92R_2.png)
