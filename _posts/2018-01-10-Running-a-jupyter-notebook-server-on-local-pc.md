---
layout: post
title: 'Running a jupyter notebook server on local pc'
date: 2018-01-10
tags: jupyter
---
$```jupyter notebook --generate-config```

> Writing default config to: /Users/osx/.jupyter/jupyter_notebook_config.py

$```jupyter notebook password```
> Enter password:11111
> Verify password:11111
> [NotebookPasswordApp] Wrote hashed password to /Users/osx/.jupyter/jupyter_notebook_config.json

$```cd cd /Users/osx/.jupyter/```
$```open .```
open jupyter_notebook_config.py
delete #, and change values as below
NotebookApp.password can find at Users/osx/.jupyter/jupyter_notebook_config.json
```
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:da4fe31bad60:f47e622c45f25a62fe308eaa700578815154f2b3'
c.NotebookApp.port = 8888
```

$```jupyter notebook```, open a new terminal tab and run the service of jupyter on the server

open url 192.168.0.51/8888 on local pc
![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/4733810/yQpRpAdfQHKvEIAptydU_1.png)
