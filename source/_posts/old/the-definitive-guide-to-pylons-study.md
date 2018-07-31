---
title: Pylons学习
tags:
  - pylons
  - python
url: 122001.html
id: 122001
categories:
  - Uncategorized
date: 2010-05-30 03:20:48
---

sqlalchemy和authkit 默认安装pylons的时候是没有安装的。因为不需要他们，Pylons也能运行。 

创建项目   
paster create -t&#160; pylons HelloWorld 

查看可用的创建项目模板   
paster create --list-template 

项目根目录下面的配置文件：   
development.ini 开发环境    
test.ini 测试环境    
还可以自己创建一个production.ini 用户生产环境 

进入项目目录   
cd HelloWorld    
运行项目    
paster serve --reload development.ini 【注意是serve，不是server，晕了我两分钟】 

paster的两个特点:   
1 源代码改变之后可以自动重新加载    
2 pylons的配置文件可以直接使用。如果使用其他的web服务器，需要PasteDeploy的支持 

ok.   
项目开始在http://localhost:5000/运行了。 

pylons项目下面的public文件夹就像是Apache的htdocs目录。放入静态文件（图片或者html文件，js,css等均可）可以直接使用。比如：   
[http://localhost:5000/hello.html](http://localhost:5000/hello.html)    
[http://localhost:5000/about/about.html](http://localhost:5000/about/about.html)

development.ini 配置节点中的host = 127.0.0.1限定了paster只是用于本机访问。   
生产环境下host=0.0.0.0 就是说任何IP都能访问（这个时候，交互式调试器已经被禁用）。 

docs目录，存放reStructredText格式的文档，可以使用Sphinx工具来将之转换成HTML格式。 

lib 目录存放controllers的公共代码，第三方库等 

websetup.py 此文件存放最终用户安装程序所需要的初始化代码。通常包括创建数据库等。 

paster controller hello 创建一个名为hello的controller.   
然后 [http://localhost:5000/hello/index](http://localhost:5000/hello/index) 就可以直接访问了，输出hello world    
这种方式与asp.net mvc差别不大。 

paster controller hello    
创建HomeController    
在config/routing.py中添加一行：    
map.connect('/', controller='home', action='index')    
然后删除public目录中的index.html （或者改名）    
表示，将项目的首页映射到HomeController/index     
访问http://localhost:5000/    
然后输出，Hello World 。替换掉了项目创建时候的首页。 

HTTP代码： 

200 ok 表示访问没问题   
401 需要验证用户身份    
403 被拒绝。没有访问权限。    
404 URL没有匹配的地址。    
500 服务发生意外。出现错误。 

截至the_definitive_guide_to_pylons P42.PDF的张数是42\. 