---
title: GAE 本地运行问题
tags:
  - GAE
  - google
url: 111001.html
id: 111001
categories:
  - Uncategorized
date: 2010-05-28 08:25:38
---

GAE 本地运行问题

KeyError: &quot;There is no item named 'simplejson\\\\_speedups.pyd' in the archive&quot;

删除python的lib\site-packages中simplejson对应的egg文件之后问题解决。

据说老早的一个问题了。google也不给修复一下。