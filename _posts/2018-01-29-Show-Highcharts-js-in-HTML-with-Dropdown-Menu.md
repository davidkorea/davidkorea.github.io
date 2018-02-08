---
layout: post
title: 'Show Highcharts.js in HTML with Dropdown Menu'
date: 2018-01-29
tags: highcharts html semanticui css
---
> All the css style provided by Semantic UI.

# 1. ui equal width grid

```HTML
<div class="ui equal width grid" style="width:75%;margin:5px 5px 5px 5px;">
	<div class="row">
		<div class="column">1  </div>    
		<div class="column">2  </div>
	</div>

	<div class="row">
		<div class="column">3  </div>
	</div>
</div>

```

![1.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/yTymOEBLSlGon69nn4QA_1.png)

# 2. Statistic

```HTML
<div class="ui red segment">
  <div class="ui statistic">
    <div class="value">
    5000
    </div>
    <div class="label">
    in total
    </div>
  </div>
</div>
```

![2.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/rA0g4zOUScmWhl149r8L_2.png)

# 3. Devided items

```HTML
<div class="ui segment">
  <div class="ui divided items">
    <div class="item">
      <div class="content">
        <div class="header">title</div>
        <div class="description">description</div>
        <div class="extra">
          <div class="ui label">tag1</div>
          <div class="ui label">tag2</div>
        </div>
      </div>
    </div>

    <div class="item">
    	... ...
    </div>
  </div>
</div>
```

![3.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/aW6rKtN6TSMoTuYhRAvp_3.png)

# 4.Highcharts.js - [Demo](https://www.highcharts.com/demo)

> JS Scripts the offical site offered is kinda confusing.
> It would be better to use the video code directly

* 1. html framework

```HTML
<div class="ui equal width grid" style="width:75%;margin:5px 5px 5px 5px;">
	... ...
</div>
<div class="ui divider"> </div>
<div class="ui equal width grid" style="width:75%;margin:5px 5px 5px 5px;">
  <div class="row">
    <div class="column">
      <div class="ui container segment">
        <div class="container" id="chart1">
        	charts       
        </div>
      </div>
    </div>
  </div>
</div>

```

![4.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/4v8j7Xc6QBqCfjkQQbbX_4.png)

* 2. import relevant js labraries & js script

The following highcharts js files can be copied from teh video code.
```HTML
<head>
<script src="js/exporting.js"></script>
<script src="js/highcharts-more.js"></script>
<script src="js/highcharts.js"></script>
</head>
```
```HTML
<body>
<div class="ui equal width grid" style="width:75%;margin:5px 5px 5px 5px;">
  <div class="row">
    <div class="column">
      <div class="ui container segment">
        <div class="container" id="chart1">
        	charts       
        </div>
      </div>
    </div>
  </div>
</div>
<script>
$ (function () {
$('#chart1').highcharts({
chart: {
type: 'bar'
},
credits:{
enabled:false
},
title: {
text: 'Fruit Consumption'
},
xAxis: {
categories: ['Apples', 'Bananas', 'Oranges']
},
yAxis: {
title: {
text: 'Fruit eaten'
}
},
series: [{
name: 'Jane',
data: [5, 7, 3]
}, {
name: 'John',
data: [5, 7, 3]
}]
});
});
</script>
</body>
```

![5.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/FEnm610OT0uJQfdsa62O_5.png)

# 5. Dropdown Menu
```HTML
<div class="ui equal width grid" style="width:75%;margin:5px 5px 5px 5px;">
  <div class="row">
    <div class="column">

      <!-- dropdown item-->    
      <div class="ui simple dropdown item">
        Menber
        <i class="dropdown icon"></i>
        <div class="menu">
        <div class="item" id="andy">Andy</div>
        <div class="item" id="david">David</div>
        </div>
      </div>
      <!-- dropdown item-->

      <div class="ui container segment">
        <div class="container" id="chart1">
            charts       
        </div>
    	</div>
    </div>
  </div>
</div>
```
![6.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/1RLenU7kSmirEdXTO6rA_6.png)

# 6. Dropdown Menu + HighCharts.js

```HTML
<div class="ui simple dropdown item">
  Menber
  <i class="dropdown icon"></i>
  <div class="menu">
    <div class="item" id="andy">Andy</div>
    <div class="item" id="david">David</div>
  </div>
</div>

<script>
$('#andy').click(function () {
$('#chart1').highcharts({
chart: {
type: 'bar'
},
credits:{
enabled:false
},
title: {
text: 'ANDY'
},
xAxis: {
categories: ['Apples', 'Bananas', 'Oranges']
},
yAxis: {
title: {
text: 'Fruit eaten'
}
},
series: [{
name: 'Jane',
data: [5, 7, 3]
}, {
name: 'John',
data: [5, 7, 3]
}]
});
});
</script>
```
> ```$('#andy').click(function () { ```
>	```$('#chart1').highcharts({ ```
> ``` ... ... ```
> ```}); ```
> ```});```
> It means when you click ```<div id="andy">```,
> the ```<div id="chart1">```will be activated(showed).

![7.png](http://user-image.logdown.io/user/42937/blog/39533/post/5404393/FmnWhA8fSNSfWdZRx7uN_7.png)
