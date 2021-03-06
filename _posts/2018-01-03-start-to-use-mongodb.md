---
layout: post
title: 'Start to use MongoDB'
date: 2018-01-03
tags: python mongodb pymongo
---
# 1.split content in lines
walden.txt
create new file in pycharm named walden.txt and copy the content
```Python
path = '/Users/osx/PycharmProjects/lesson2/waldenn.txt'
with open(path, 'r') as f:
    lines = f.readlines()
    for index, line in enumerate(lines):
        data = {
            'index':index,
            'line' :line,
            'words':len(line.split())
        }
        print(data)
```

if open a text file with ascii or unicode or utf8 python can not decode

> Error:
	UnicodeDecodeError: 'ascii' codec can't decode byte 0xff in position 0: ordinal not in range(128)
	Process finished with exit code 1


# 2. Save splited lines into Mongodb
```Python
import pymongo

client = pymongo.MongoClient('localhost', 27017)
workbook = client['workbook']
sheet_tab = workbook['sheet_tab']

path = '/Users/osx/PycharmProjects/lesson2/waldenn.txt'
with open(path, 'r') as f:
    lines = f.readlines()
    for index, line in enumerate(lines):
        data = {
            'index':index,
            'line' :line,
            'words':len(line.split())
        }
        sheet_tab.insert_one(data)
```
if no pymongo package in this project, add it as follow

![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/4716513/5HG7fzZTRg2M3j5VF6GF_3.png)

still, important thing! turn on mongodb service as follow command in iTerm
```
mongod
```

# 3.check the data saved in db
## 3.1 use code
```Python
import pymongo

client = pymongo.MongoClient('localhost', 27017)
workbook = client['workbook']
sheet_tab = workbook['sheet_tab']

# path = '/Users/osx/PycharmProjects/lesson2/waldenn.txt'
# with open(path, 'r') as f:
#     lines = f.readlines()
#     for index, line in enumerate(lines):
#         data = {
#             'index':index,
#             'line' :line,
#             'words':len(line.split())
#         }
#         sheet_tab.insert_one(data)

for item in sheet_tab.find():
    print(item)
```
## 3.2 use plug-in tool in pycharm
![4.png](http://user-image.logdown.io/user/42937/blog/39533/post/4716513/77ixpyQ8Soa0zDscR6r1_4.png)


![5.png](http://user-image.logdown.io/user/42937/blog/39533/post/4716513/hRCiltuQPCs0AcQ5AQsw_5.png)

> it seems that the plug-in tool can only show 300 line records????????????
> using command can find all the records in mongodb
> 2018-1-10

# 4 orgnize data in order
## 4.1 fliter commands/query db
    >  $lt/$lte/$gt/$gte/$ne，依次等价于</<=/>/>=/!=。（l表示less g表示greater e表示equal n表示not ）
example:
```Python
for item in sheet_tab.find({'words':{'$lt':5}}):
print(item)
#find the words of a line is less than 5 words
```
