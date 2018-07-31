---
title: micolog
tags:
  - micolog
url: 64001.html
id: 64001
categories:
  - Uncategorized
date: 2010-05-13 03:54:54
---

一直看[micolog](http://micolog.xuming.net)的模板不爽，终于花了点时间把[呼呼堂](http://www.small-island.org/)的模板拔下来。现在看着点还像回事。

改了几处micolog的代码，记一下。免得下次代码升级忘记了：

- blog.py中的m_list_pages 读取页面列表前后都有&lt;li&gt;标签写死了，删除之，直接用空格分开。

- 获取Archives的函数给写死只读取12条记录，archives=Archive.all().order('-year').order('-month')#.fetch(12)

- 后台设置中的，默认结构化链接和结构化链接(Permalink)两个项目不明，修改不了。

- Page的地址竟成了[/2008/05/6/about.html](http://d9.appspot.com/2008/05/6/about.html "http://d9.appspot.com/2008/05/6/about.html") 这种，不爽。

- micolog现在可算是最好的GAE博客程序了。插件和模板体制都差不多完善了。但是可以改进的地方还是很多的，就如上面说的函数调用的灵活性方面还有待加强。

- 模板亟需能上传，就不用改一次模板就要重新上传整个程序了。

- 程序更新方面，能在线更新就好了

- 一个算不上BUG的大问题，设置了一个模板之后，比如次模板名为ta;然后重新上传程序的时候，ta这个模板被删除了。然后在访问网站，会发现整站崩溃了。这种情况下是不是可以判断一下模板是否存在，否则读取默认或者提示用户重新设置模板。

令，wp导入数据的两个后遗症：

- postname中出现中文编码的地方会出现404

- 原有文章中需要p,br标签分开的地方都没加。导致现在无格式。