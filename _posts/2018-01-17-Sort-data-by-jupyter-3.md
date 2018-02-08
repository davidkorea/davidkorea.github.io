---
layout: post
title: 'Sort data by jupyter 3'
date: 2018-01-17
tags: jupyter aggregate pipeline
---
### Sort data by using [aggregate(pipeline)->user guide](https://docs.mongodb.com/manual/reference/operator/aggregation/)

# 1.Overview of aggregate
Aggregate function need the pipeline to sort data where it contains all the conditions we need.
first, let's get to know how to write the sort conditions.
```python
pipeline = [
    {'$match': {'$and':[{'date':'2018/01/09'},{'handle_by':'David Liu'}]}},
    {'$group': {'_id':'$city','counts':{'$sum':1}}},
    {'$sort': {'counts':-1}},
    {'$limit': 3}
]
for i in loginfo.aggregate(pipeline):
    print(i)
```

* ```$match```: ```$and```is needed if 2 conditions must be met at the same time,if not```{'$match':{'date':'2018/01/09'}}```

* ```$group```: grouping by 'city' and will sum up the same city +1, 'counts' isn't in database but named manually

* ```$sort```: -1: from 9 to 0, 1: from 0 to 9

* ```$limit```: same as ```find().limit(3)``` , show items by limit(3)

![1.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4773093/PhwpK2GQRRatsPLZZsXp_1.jpg)
> city column chart top 10

# 2.Transfer it to be a data generator for pie charts
```python
 def data_gen(date, member):
    pipeline = [
        {'$match':{'$and':[{'date':date},{'handle_by':member}]}},
        {'$group':{'_id':'$city', 'counts':{'$sum':1}}},
        {'$sort': {'counts':-1}},
    ]
    for i in loginfo.aggregate(pipeline):
        yield [i['_id'], i['counts']]
```
when we use ```yield```, a list could be yield by puting data in '```[]```',like ```yield [i['_id'], i['counts']]```
![2.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4773093/31dZ16OqQ8WJgeVYfGP7_2.jpg)

# 3. Make a pie chart
```python
options = {
    'chart':{'zoomType':'xy'},
    'title':{'text':'city on one day'}
}
series = [{
    'type': 'pie',
    'name': 'volume',
    'data': [i for i in data_gen('2018/01/09','David Liu')]
}]

charts.plot(series, options=options, show='inline')
```

![3.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4773093/SAQX7PO8T3KsT40yUp3m_3.jpg)

-----
> ### More...
# Q: 2018/01/01-2018/01/09 city volume in a period

```python
pipeline = [
    {'$match': {'$and': [{'date':{'$gte':'2017/01/01','$lte':'2018/01/09'}},{'handle_by':'David Liu'}]}},
    {'$group': {'_id':'$city', 'counts': {'$sum': 1}}},
    {'$sort':{'counts':-1}},
    {'$limit':10}
]
for i in loginfo.aggregate(pipeline):
    print(i)
```
unduplicated data can be sorted through ```$grooup```, cuz it means that find the same city and add 1.
grouping means take duplicated data in order and make it unduplicated.

![2.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4773093/fyHR4bURfabsCNCZ17iy_2.jpg)
create a data generator to yiled the data that column chart needs.
```python
def data_gen(date1, date2, member, limit):
    pipeline = [
    {'$match': {'$and': [{'date':{'$gte':date1,'$lte':date2}},{'handle_by':member}]}},
    {'$group': {'_id':'$city', 'counts': {'$sum': 1}}},
    {'$sort': {'counts':-1}},
    {'$limit': limit}
]
    for i in loginfo.aggregate(pipeline):
        data = {
            'name': i['_id'],
            'data': [i['counts']],
            'type': 'column'
        }
        yield data
```
then use the data yielded above to generate the chart
```python
series = [i for i in data_gen('2017/01/01','2017/12/31','David Liu',10) ]
options = {
    'chart':{'zoomType':'xy'},
    'title':{'text':'Top 10 Citys Handled by David in 2017'},
    'yAxis':{'title':{'text':'volume'}},
    }
charts.plot(series, options=options, show='inline')
```

![3.jpg](http://user-image.logdown.io/user/42937/blog/39533/post/4773093/lppeh9SiRHGDIfJDAsQw_3.jpg)
