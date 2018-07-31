---
title: '[(''a'', 3), (''b'', 1), (''c'', 2)] 按照第二项排序'
tags:
  - python
url: 640837.html
id: 640837
categories:
  - 日常。笔记
date: 2009-07-20 01:56:10
---

<pre>

"""
[('a', 3), ('b', 1), ('c', 2)] 按照中间元组的第二项排序
"""
def my_cmp(E1, E2):
    return cmp(E1[1], E2[1]) #compare weight of each 2-tuple
    #return the negative result of built-in cmp function
    #thus we get the descend order
L = [('a',3),('b',1),('c',2)]
L.sort(my_cmp)
print L
list = [('a', 3), ('b', 1), ('c', 2)]
list.sort( key=lambda x: x[1] )
print list

</pre>