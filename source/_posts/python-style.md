---
title: python 编程风格
tags:
  - python
url: 116001.html
id: 116001
categories:
  - Uncategorized
date: 2010-05-29 07:18:33
---

一个典型模块的内部结构： 

1.  起始行2.  模块文档(文档字符串)3.  模块导入4.  (全局)变量定义5.  类定义(若有)6.  函数定义(若有)7.  主程序  

示例代码：
  <pre>#!/usr/bin/env python
#coding:utf-8

&quot;this is a test module&quot;

import sys
import os

debug = True

class FooClass(object):
    &quot;Foo class&quot;
    pass

def test():
    &quot;test function&quot;
    foo = FooClass()
    if debug:
        print 'ram test()'

if __name__ == '__main__':
    test()</pre>

&#160;

参考：google的python编程规范 [Google Python Style Guide](http://google-styleguide.googlecode.com/svn/trunk/pyguide.html)