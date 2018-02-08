---
layout: post
title: 'Get a Week Period'
date: 2018-02-01
tags: python datetime
---
> Get the date period of this week and last week.```from datetime import *```
date.weekday()：Return weekday，Monday，return 0；Tuesday，return 1
data.isoweekday()：Return weekday，Monday，return 1；Tuesday，return 2
[Reference1](http://blog.csdn.net/xinke453/article/details/51886512), [Reference2](https://www.cnblogs.com/cindy-cindy/p/6720196.html)

# 1. Today

```python
from datetime import *

def current_week(today=date.today()):
    weekday = today.isoweekday()
    if weekday == 7:
        first_day = today - timedelta(days=6)
        last_day = today - timedelta(days=2)
        print(first_day, last_day)
    if weekday == 6:
        first_day = today - timedelta(days=5)
        last_day = today - timedelta(days=1)
        print(first_day, last_day)
    if weekday == 5:
        first_day = today - timedelta(days=4)
        last_day = today
        print(first_day, last_day)
    if weekday == 4:
        first_day = today - timedelta(days=3)
        last_day = today + timedelta(days=1)
        print(first_day,last_day)
    if weekday == 3:
        first_day = today - timedelta(days=2)
        last_day = today + timedelta(days=2)
        print(first_day,last_day)
    if weekday == 2:
        first_day = today - timedelta(days=1)
        last_day = today + timedelta(days=3)
        print(first_day,last_day)
    if weekday == 1:
        first_day = today
        last_day = today + timedelta(days=4)
        print(first_day,last_day)

def last_week(today=date.today()):
    weekday = today.isoweekday()
    if weekday == 7:
        first_day = today - timedelta(days=13)
        last_day = today - timedelta(days=9)
        print(first_day, last_day)
    if weekday == 6:
        first_day = today - timedelta(days=12)
        last_day = today - timedelta(days=8)
        print(first_day, last_day)
    if weekday == 5:
        first_day = today - timedelta(days=11)
        last_day = today - timedelta(days=7)
        print(first_day, last_day)
    if weekday == 4:
        first_day = today - timedelta(days=10)
        last_day = today - timedelta(days=6)
        print(first_day,last_day)
    if weekday == 3:
        first_day = today - timedelta(days=9)
        last_day = today - timedelta(days=5)
        print(first_day,last_day)
    if weekday == 2:
        first_day = today - timedelta(days=8)
        last_day = today - timedelta(days=4)
        print(first_day,last_day)
    if weekday == 1:
        first_day = today - timedelta(days=7)
        last_day = today - timedelta(days=3)
        print(first_day,last_day)

current_week()
last_week()
```

# 2. Oneday

```python
from datetime import *

def current_week(day):
    today = date(int(day.split('/')[0]),int(day.split('/')[1]),int(day.split('/')[2]))
    weekday = today.isoweekday()
    if weekday == 7:
        first_day = today - timedelta(days=6)
        last_day = today - timedelta(days=2)
        print(first_day, last_day)
    if weekday == 6:
        first_day = today - timedelta(days=5)
        last_day = today - timedelta(days=1)
        print(first_day, last_day)
    if weekday == 5:
        first_day = today - timedelta(days=4)
        last_day = today
        print(first_day, last_day)
    if weekday == 4:
        first_day = today - timedelta(days=3)
        last_day = today + timedelta(days=1)
        print(first_day,last_day)
    if weekday == 3:
        first_day = today - timedelta(days=2)
        last_day = today + timedelta(days=2)
        print(first_day,last_day)
    if weekday == 2:
        first_day = today - timedelta(days=1)
        last_day = today + timedelta(days=3)
        print(first_day,last_day)
    if weekday == 1:
        first_day = today
        last_day = today + timedelta(days=4)
        print(first_day,last_day)


def last_week(day):
    today = date(int(day.split('/')[0]),int(day.split('/')[1]),int(day.split('/')[2]))
    weekday = today.isoweekday()
    if weekday == 7:
        first_day = today - timedelta(days=13)
        last_day = today - timedelta(days=9)
        print(first_day, last_day)
    if weekday == 6:
        first_day = today - timedelta(days=12)
        last_day = today - timedelta(days=8)
        print(first_day, last_day)
    if weekday == 5:
        first_day = today - timedelta(days=11)
        last_day = today - timedelta(days=7)
        print(first_day, last_day)
    if weekday == 4:
        first_day = today - timedelta(days=10)
        last_day = today - timedelta(days=6)
        print(first_day,last_day)
    if weekday == 3:
        first_day = today - timedelta(days=9)
        last_day = today - timedelta(days=5)
        print(first_day,last_day)
    if weekday == 2:
        first_day = today - timedelta(days=8)
        last_day = today - timedelta(days=4)
        print(first_day,last_day)
    if weekday == 1:
        first_day = today - timedelta(days=7)
        last_day = today - timedelta(days=3)
        print(first_day,last_day)

current_week('2018/02/18')
last_week('2018/02/18')
```
