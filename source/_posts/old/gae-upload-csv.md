---
title: GAE使用CSV格式上传数据
tags:
  - GAE
  - python
url: 91001.html
id: 91001
categories:
  - Uncategorized
date: 2010-05-27 08:40:52
---

首先,在app.yaml中添加
  > - url: /remote_api     
> &nbsp; script: $PYTHON_LIB/google/appengine/ext/remote_api/handler.py      
> &nbsp; login: admin  

添加test_loader.py
  > import datetime     
> from google.appengine.ext import db      
> from google.appengine.tools import bulkloader 
> 
> class Test(db.Model):     
> &nbsp; id = db.IntegerProperty()      
> &nbsp; name = db.StringProperty()      
> &nbsp; desc = db.StringProperty() 
> 
> class TestLoader(bulkloader.Loader):     
> &nbsp; def __init__(self):      
> &nbsp;&nbsp;&nbsp; bulkloader.Loader.__init__(self, 'Test',      
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [('id', int)      
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ,('name', str)      
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ,('desc',lambda x: unicode(x, "utf-8"))      
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ]) 
> 
> loaders = [TestLoader]  

上传批处理命令
  > cd C:\Program Files\Google\google_appengine     
> appcfg.py upload_data --config_file=test_loader.py --filename=test.csv --kind=Test --auth_domain=xxx.com --url=http://www.xxx.com/remote_api D:\xxx  

尝试尝试在尝试。终于好了。基本上是按照官方文档来的，但是不管是不小心还是咋滴，总会有问题。但是所幸，功夫不负有心人！！