---
layout: post
title: 'Get a Month Period'
date: 2018-02-02 07:07
comments: true
categories: 
---
> This article is aiming at getting the first day and the last day of current week and last week.
> ```from datetime import *```
> ```import calendar```

> when you use ```month_range = calendar.monthrange(year, month)```, it returns a tuple (x, y)
x = the 요일 of the month's first day
y = the total days in the month
Be careful when you use the attribute y by using ```month_range[1]```

[Reference1](http://python3-cookbook.readthedocs.io/zh_CN/latest/c03/p14_date_range_for_current_month.html), [Reference2](http://blog.csdn.net/yannanxiu/article/details/53809600) 


# 1. today 

```python
from datetime import *
import calendar

def current_month(today = date.today()):
    year = today.year
    month = today.month
    month_range = calendar.monthrange(year,month)
    first_day = date(year, month, day=1)
    last_day = date(year, month, day=month_range[1])
    print(first_day,last_day)

def last_month(today=date.today()):
    year = today.year
    month = today.month
    if month == 1:
        last_year = year -1
        first_day = date(last_year, 12, 1)
        last_day = date(last_year, 12, 31)
        print(first_day, last_day)
    else:
        month_range = calendar.monthrange(year, month-1)
        first_day = date(year,month-1, 1)
        last_day = date(year, month-1, month_range[1])
        print(first_day, last_day)

current_month()
last_month()
```

# 2. oneday

```python
from datetime import *
import calendar

def current_month(day):
    today = date(int(day.split('/')[0]),int(day.split('/')[1]),int(day.split('/')[2]))
    year = today.year
    month = today.month
    month_range = calendar.monthrange(year,month)
    first_day = date(year, month, day=1)
    last_day = date(year, month, day=month_range[1])
    print(first_day,last_day)

def last_month(day):
    today = date(int(day.split('/')[0]),int(day.split('/')[1]),int(day.split('/')[2]))
    year = today.year
    month = today.month
    if month == 1:
        last_year = year -1
        first_day = date(last_year, 12, 1)
        last_day = date(last_year, 12, 31)
        print(first_day, last_day)
    else:
        month_range = calendar.monthrange(year, month-1)
        first_day = date(year,month-1, 1)
        last_day = date(year, month-1, month_range[1])
        print(first_day, last_day)


current_month('2018/01/03')
last_month('2018/01/11')
```

