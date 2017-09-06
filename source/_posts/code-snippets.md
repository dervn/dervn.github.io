---
title: 几个代码片段
tags:
  - JQuery
  - JS
url: 640880.html
id: 640880
categories:
  - 日常。笔记
date: 2009-09-07 09:32:32
---

- 默认图片
<pre>![](/images/DMA001.jpg)</pre>
图片不存在，则使用默认图片替换。

- JQuery判断页面元素是否隐藏
<pre>if($("#main-fl").is(":hidden"))   {//。。。。}</pre>

- 多行某列值相加

id name
1  a
2  b
3  c
4  d
5  e
6  f

sql一个表 有 ID ,NAME 两列    怎么样写语句  结果为    abcdef

解答：
<pre>DECLARE @str AS VARCHAR(5000)
SELECT @str = ''
SELECT @str = @str + LTRIM(name)
FROM   T
SELECT @str</pre>