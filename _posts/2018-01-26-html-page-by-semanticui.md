---
layout: post
title: 'HTML Page by SemanticUI'
date: 2018-01-26 04:48
comments: true
categories: [semanticui]
---
# 1. Preparation

* Create project folder
* Copy semantic css,js,fonts,images to project folder
* Atom open project
* New file - index.html

# 2. HTML Framework

```html
  <head>
    <meta charset="utf-8">
    <title>web</title>
    <link rel="stylesheet" href="css/semantic.css">
    <script src="js/jquery.min.js"></script>
    <script src="js/semantic.js"></script>
  </head>
```

> jquery.min.js should be above of semantic.js

```html
  <body>    
    <div class="ui menu">
      <div class="header item" id="menu">
        Menu
        <i class="content icon"></i>
      </div>
      <div class="item">about us</div>
      <div class="item">others</div>
    </div>

    <div class="ui text container segment">
      
      <div class="ui divided items">
        <div class="item">
          <div class="content">
            <div class="header">
              title
            </div>
            <div class="description">
              <p>something</p>
            </div>
            <div class="extra">
              <div class="ui label">
                tag1
              </div>
            </div>
          </div>
        </div>

      <div class="ui pagination menu">
        <a href="" class="active item"><</a>
        <div class="disable item">
          page 1of 3
        </div>
        <a href="#" class="item">></a>
      </div>      
    </div>    
  </body>
```

> For fonts being used normally, semantic.css should be edited as follows.
> ```../fonts/```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5395351/ps6D6CsTqeHmIqMrl5GQ_2.png)

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5395351/cQ7wyVJR0CBWXoRJNuAT_1.png)

# 3. JS - Sidebar

```html
<body>
  <div class="ui sidebar Inverted vertical menu">
    here is the sidebar
  </div>
  <div class="pusher">
    here is the normal html content page,
    which contains all the codes above
  </div>    
  
  <script>
      $('#menu').click(function () {
        $('.ui.sidebar').sidebar('toggle')
      })
   </script>
  
</body>
```
Now, let's jump into the codes
```html
  <body>
    <div class="ui visible left thin sidebar inverted vertical menu" id="sidebar">
      <div class="item">option1</div>
      <div class="item">option2</div>
    </div>

    <div class="pusher">
    	<div class="ui menu">
        <div class="ui text container segment">
    </div>

    <script>
      $('#menu').click(function () {
        $('#sidebar').sidebar('toggle')
      })
    </script>
  </body>
```

> ``` <div class="ui visible left thin sidebar inverted vertical menu" id="sidebar">```
> 'left' is a must, otherwise the sidebar would on the normal page, which means the normal page can't be seen

![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/5395351/JKycG2YRTqGbwfXZL7Hv_3.png)
