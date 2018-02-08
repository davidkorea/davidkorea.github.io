---
layout: post
title: 'Sort data by jupyter 2'
date: 2018-01-13
tags: jupyter highcharts
---
# 1.get the data we need by .find({},{})
to find the only data that i need through monggodb find() ,find(),will not change database.
find the data in 2018/01/09 and only show caseid,```loginfo.find({'date':'2018/01/09'},{'_id':0,'caseid':1})```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/WPfwEzTxmpscbpl3qiGw_1.png)

to find data in a range,use $in,``` loginfo.find({'date',{'$in':['2018/01/08','2018/01/09']}},{'_id':0,'date':1,'caseid':1})```


![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/w5JgGHn6QaoOf29r7XAw_2.png)

the different format of data,yy-mm-dd,yy.mm.dd,yy/mm/dd
```Python
for i in loginfo.find():
    frags = i['date'].split('/')
    if len(frags) == 1:
        date = frags
    else:
        date = '{}.{}.{}'.format(frags[0],frags[1],frags[2])
    print(date)
    loginfo.update_one({'_id':i['_id']},{'$set':{'date':date}})
```
![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/OZ25yEvnRvGcwQDhpTsr_3.png)

# 2. let codes know the date

import the libraries, ```from datetime import timedelta, date```
firstly, let's ge to know how date and datedelta work
```Python
a = date(2018,1,13)
print(a)
```
```Python
a = timedelta(days=1)
print(a)
```
![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/NnfetC86QleEvKoEUIWV_1.png)
NOW, let's jump into the codes
unfortunately, this is a wrong case to remind me not to do it again

![1.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/OubwIE4KTHeR3J7a4q0t_1.jpg)
we will input the date like '2018-01-01' into the get_all_dates(). and for comparing the 2 dates, we need to split it first, and the combine them together by date() so that our code could compare them and give us a date period. BUT, when use date() to combine, transfer it into ```int(date1.split('-'))```.

you may think why not use date2 minus date1???. the problem is how to minus them and give us how many days between them???
```Python
def get_all_dates(date1, date2):
    start_date = date(int(date1.split('/')[0]),int(date1.split('/')[1]),int(date1.split('/')[2]))
    end_date = date(int(date2.split('/')[0]),int(date2.split('/')[1]),int(date2.split('/')[2]))
    days = timedelta(days=1)
    while start_date <= end_date:
        yield(start_date.strftime('%y/%m/%d'))
        start_date = start_date + days
```
```Python
for i in get_all_dates('2018/01/01','2018/01/10'):
    print(i)
```

![2.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/Jswq4hMPTJyw6Bj9Yykz_2.jpg)

you probably find that the date of the year is %yy, not %yyyy
so, just change the code to ```yield (start_date.strftime('%Y-%m-%d'))```, capital letter Y.

![1.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/eCKNFOV7QjeKyj9pEsa9_1.jpg)

# 3. find the member & the date

we need to find the volume of which member on which day.

```Python
def get_data_within(date1,date2,members):
    for member in members:
        for date in get_all_dates(date1,date2):
            a = list(loginfo.find({'date':date,'handle_by':member}))
            print(date, member, len(a))
```
```Python
get_data_within('2018/01/01','2018/01/10',['David Liu'])
```
we use ```for``` loop to find who in which day did how much cases. that means each factor in members, and get_date_from() will generate a list named a, one by one.
![2.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/A8dVcnW2Q3O83Oer3dqV_2.jpg)

# 4.Make the chart by using the data above

```Python
def get_data_within(date1, date2, members):
    for member in members:
        each_day_posts = []
        for date in get_all_dates(date1,date2):
            a = list(loginfo.find({'date':date, 'handle_by':member}))
            each_day_post = len(a)
            each_day_posts.append(each_day_post)
        data = {
            'name': member,
            'data': each_day_posts,
            'type': 'line'
        }
        yield data
```
```Python
for i in get_data_within('2018/01/01', '2018/01/10', ['David Liu','Andy Tsao']):
    print(i)
```
![1.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/970Qf0UTCLqpdBEeRk4Q_1.jpg)

```Python
options = {
    'chart': {'zoomType':'xy'},
    'title' : {'text': 'volume'},
    'subtitle' : {'text': 'by member'},
    'xAxis' : {'categories': [i for i in get_all_dates('2018/01/01', '2018/01/10')]},
    'yAxis' : {'title': {'text': 'cases'}}
}
series = [i for i in get_data_within('2018/01/01', '2018/01/10', ['David Liu','Andy Tsao'])]
charts.plot(series, options=options, show='inline')
```

![1.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4739924/HjXtvhUSSmiTTM42uV3d_1.jpg)
