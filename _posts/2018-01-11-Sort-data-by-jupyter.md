---
layout: post
title: 'Sort data by jupyter'
date: 2018-01-11
tags: jupyter chart
---
# 1. make a backup of database
1st, open a terminal in jupyter
$ ```mongod```
$ ```mongo```
2nd, select the db you will use
$ ```show dbs```
$ ```use awms```
3rd, select which table you will use
$ ```show tables```
4th, create a backup table and copy the data from orgin one to the one newly created
$ ```db.createCollection('loginfo_backup')```
$ ```db.loginfo.copyTo('logdata_backup')```
5th, use backup db when sort data
```python
import pymongo
client = pymongo.MongoClient('localhost', 27017)
awms = client['awms']
loginfo = awms['loginfo_backup']
```

# 2. String of '\xa0' in ['caseid']

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/FlGFz0ZTQmFaPfkbU1p7_1.png)
1st, use this codes to find out which one has '\xa0'
```python
for i in loginfo.find():
    if '\xa0' in i['caseid']:
        print(i['caseid'])
```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/v9hToJhOSBOakpFsWitF_2.png)

2nd, delete this string of data, you need to ```j = "".join(j.split())```,or ```j = ''.join(j.split())```
refer to :[https://www.cnblogs.com/yqpy/p/8203783.html](https://www.cnblogs.com/yqpy/p/8203783.html)
```python
for i in loginfo.find():
    if '\xa0' in i['caseid']:
        j = i['caseid']
        j = "".join(j.split())
        if '\xa0' in j:
            print('yes')
        else:
            print('no')
```

![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/ygx6Co5RWexSkayag1CJ_3.png)

3rd, update the refreshed data to db
```python
for i in loginfo.find():
    if '\xa0' in i['caseid']:
        j = i['caseid']
        j = ''.join(j.split())
        loginfo.update({'_id':i['_id']},{'$set':{'caseid':j}})
```
![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/BxAQSs23SmOKiV1YY2Gv_1.png)

# 3. Make data visible by highcharts

import charts package
```python
import charts
```
shift + enter, it runs well if the following come out‘Server running in the folder /Users/osx at 127.0.0.1:65060’

firstly, let's see a sample

```python
series = [{
    'name': 'OS X',
    'data': [11],
    'type': 'column'
},{
    'name': 'windows',
    'data': [12],
    'type': 'column'
},{
    'name': 'other',
    'data': [2],
    'type': 'column'
}]
charts.plot(series, show='inline', options=dict(title=dict(text='charts are nice!')))
```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/UxyPOH4GTHygDqx9hiV6_2.png)

NOW, let's start to make a chart to show the volume of each city
1st, unduplicated city names
```python
city_list = []
for i in loginfo.find():
    city_list.append(i['city'])
city_index = list(set(city_list))
print(city_index)
```
    # unduplicated city索引, 集合set可自动去重
![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/i4cAurYwR3GMDsjSbT4U_3.png)

2nd, volumes of each unduplicated city
```python
post_times = []
for index in city_index:
    post_times.append(city_list.count(index))
print(post_times)
```
    # count each unduplicated city in the duplicated city_list

![4.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/hDSSv4gMTSOsA4ci2viQ_4.jpg)

 3rd, make a data generator for making a chart
```python
def data_gen(types):
    length = 0
    if length <= len(city_index):
        for city, times in zip(city_index, post_times):
            data = {
                'name': city,
                'data': [times],
                'type': types
            }
            yield data
            length += 1
```
![5.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/gL5IFhd8TEauzgBQ4FuR_5.jpg)

4th, make the chart
```python
series = [data for data in data_gen('column')]
charts.plot(series, show='inline', options=dict(title=dict(text='case city')))
```

![6.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4734411/ACLdbUQRlKTFhl9BdfIP_6.jpg)
