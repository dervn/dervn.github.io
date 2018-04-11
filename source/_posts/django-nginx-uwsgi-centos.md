---
title: 'Django项目部署'
categories:
  - 技术
date: 2018-03-23
tags:
  - python
  - Django
  - CentOS7
---

Django项目部署

## Centos7安装Python3的方法(与python2共存)
1. 依赖包安装

    ```
    yum -y groupinstall "Development tools"
    yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
    ```

2. 源文件安装python3

    ```
    # 下载源文件，可根据需要下载不同版本
    wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tar.xz
    mkdir /usr/local/python3
    # 解压，安装
    tar -xvJf  Python-3.6.4.tar.xz
    cd Python-3.6.4
    ./configure --prefix=/usr/local/python3
    make && make install
    # 创建软连接
    ln -s /usr/local/python3/bin/python3 /usr/bin/python3
    ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
    ```
参考链接 https://www.cnblogs.com/FZfangzheng/p/7588944.html

## CentOS7 yum 安装Nginx
1. 安装和启动
    ```
    # 安装
    sudo yum install -y nginx
    # 启动运行
    sudo systemctl start nginx.service
    # 设置开机启动
    sudo systemctl enable nginx.service
    ```
2. 相关目录

    ```
    # 网站默认路径 /usr/share/nginx/html
    # 默认站点配置 /etc/nginx/conf.d/default.conf
    # 自定义Nginx站点配置文件存放目录 /etc/nginx/conf.d/
    # Nginx全局配置 /etc/nginx/nginx.conf
    # Nginx启动 nginx -c nginx.conf
    ```
3. 自定义站点CONF

    ```
    server {
        listen 80;
        server_name www.jiushupuzi.com;
        charset     utf-8;
        access_log      /usr/share/nginx/html/baijiacaipu/log/nginx_access.log;
        error_log       /usr/share/nginx/html/baijiacaipu/log/nginx_error.log;
        client_max_body_size 75M;


        location /assets {
            alias /usr/share/nginx/html/baijiacaipu/assets;
        }

        location / {
            include     /etc/nginx/uwsgi_params;
            uwsgi_pass  127.0.0.1:8000;
        }
    }
    ```
4. 建立软连接

    ```
    sudo ln -s /usr/share/nginx/html/baijiacaipu/conf/nginx.conf /etc/nginx/conf.d/baijiacaipu.conf
    ```

## 数据库安装 MariaDB
1. 安装和启动

    ```
    # 安装
    yum -y install mariadb mariadb-server
    # 启动
    systemctl start mariadb
    # 设置开机启动
    systemctl enable mariadb
    # 简单引导配置
    mysql_secure_installation
    # 完成
    mysql -uroot -p
    ```

2. MariaDB字符集配置
    * 文件/etc/my.cnf，在[mysqld]标签下添加

        ```
        init_connect='SET collation_connection = utf8_unicode_ci'
        init_connect='SET NAMES utf8'
        character-set-server=utf8
        collation-server=utf8_unicode_ci
        skip-character-set-client-handshake
        ```

    * 文件/etc/my.cnf.d/client.cnf,在[client]中添加

        ```
        default-character-set=utf8
        ```

    * 文件/etc/my.cnf.d/mysql-clients.cnf, 在[mysql]中添加

        ```
        default-character-set=utf8
        ```
3. 重启命令

    ```
    systemctl restart mariadb
    ```

4. 字符集查看SQL

    ```
    show variables like "%character%";
    show variables like "%collation%";
    ```
5. 用户创建和权限授权

    ```
    # 创建用户命令
    create user username@localhost identified by 'password';
    # 直接创建用户并授权的命令
    grant all on *.* to username@localhost indentified by 'password';
    # 授予外网登陆权限
    grant all privileges on *.* to username@'%' identified by 'password';
    # 授予权限并且可以授权
    grant all privileges on *.* to username@'hostname' identified by 'password' with grant option;
    #其中只授予部分权限把其中all privileges或者all改为select,insert,update,delete,create,drop,index,alter,grant,references,reload,shutdown,process,file其中一部分。
    ```

## uwsgi
1. 安装

    ```
    sudo yum install python-devel gcc
    sudo pip3 install uwsgi
    ```
2. 配置uwsgi.ini

    ```
    [uwsgi]
    socket = 127.0.0.1:8000
    chdir=/usr/share/nginx/html/baijiacaipu
    module=baijiacaipu.wsgi
    master = true
    processes=2
    threads=2
    max-requests=2000
    chmod-socket=664
    vacuum=true
    daemonize = /usr/share/nginx/html/baijiacaipu/log/uwsgi.log
    ```
3. 执行

    ```
    /usr/local/python3/bin/uwsgi --ini /usr/share/nginx/html/baijiacaipu/conf/uwsgi.ini
    ```

4.  nginx中uwsgi配置文件路径

    ```
    /etc/nginx/uwsgi_params
    ```

## supervisor
1. 当前3.x版本尚不支持python3,但是可以在python2下运行supervisor【此时代码可以是2也可以是3，不影响】
2. 安装

    ```
    sudo pip install supervisor
    ```

3. 配置文件

    ```
    ;[include]
    ;files = /relative/dictory/*.ini

    [program:baijiacaipu]
    command=/usr/local/python3/bin/uwsgi --ini /usr/share/nginx/html/baijiacaipu/conf/uwsgi.ini
    directory=/usr/share/nginx/html/baijiacaipu/
    startsecs=0
    stopwaitsecs=0
    autostart=true
    autorestart=true
    stdout_logfile=/usr/share/nginx/html/baijiacaipu/log/supervisord_stdout.log
    stderr_logfile=/usr/share/nginx/html/baijiacaipu/log/supervisord_stderr.log

    [supervisord]
    ```

4. 启动，通过supervisord管理启动和配置supervisor本身

    ```
    supervisord -c /usr/share/nginx/html/baijiacaipu/conf/supervisord.conf
    ```

5. 通过supervisorctl来管理使用supervisor启动和管理的自身的一些应用

    ```
    supervisorctl shutdown # 关闭
    supervisorctl start program_name #启动应用
    supervisorctl reload #重启
    # 查看supervisor运行状态
    ps -efH|grep supervisor
    ```

## CentOS开启zsh
1. 安装zsh

    ```
    sudo yum install zsh
    cat /etc/shells
    chsh -s /bin/zsh
    echo $SHELL
    # 重启系统之后生效
    reboot
    ```
2. 安装oh-my-zsh

    ```
    wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
    # 配置 .zshrc
    # 增加别名 alias ll='ls -l'
    ```

## 其他
1. sys.setdefaultencoding(“utf-8”) 这种方式在3.x中被彻底遗弃

    https://stackoverflow.com/questions/3828723/why-should-we-not-use-sys-setdefaultencodingutf-8-in-a-py-script

2. Nginx下Django Admin界面Css、JS丢失问题解决方法

    settings.py 中设置STATIC_ROOT路径之后执行命令 python manage.py collectstatic，会生成静态文件到目录中

3. CentOS下使用yum安装pip

    ```
    sudo yum -y install epel-release
    sudo yum -y install python-pip
    ```

4. Provide a simpler way to default runserver IP/port to 0.0.0.0:8000

    ```
    python manage.py runserver 0.0.0.0:8000
    ```

5. pip install error 在Python package下载中遇到ReadTimeoutError: HTTPSConnectionPool该怎么办

    * 国内镜像
        ```
        pip install -U virtualenv --index https://mirrors.ustc.edu.cn/pypi/web/simple/
        ```
    * 到https://pypi.python.org/simple/pip/下载最新的.whl文件（如pip-8.1.2-py2.py3-none-any.whl，注意：列表并非按发布时间排序，自己按文件名找到最新.whl文件）
        ```
        pip install (path)/pip-8.1.2-py2.py3-none-any.whl        
        ```

## 参考链接
1. Centos7安装Python3的方法  https://www.cnblogs.com/FZfangzheng/p/7588944.html
2. CentOS 7 yum 安装 Nginx  https://blog.csdn.net/u012486840/article/details/52610320
3. CentOS 7.0 使用 yum 安装 MariaDB 与 MariaDB 的简单配置 https://www.linuxidc.com/Linux/2016-03/128880.htm
4. centos开启zsh之旅  https://my.oschina.net/shyann/blog/426004
5. Setting up Django and your web server with uWSGI and nginx  https://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html
6. uWSGI+django+nginx的工作原理流程与部署历程
 https://blog.csdn.net/c465869935/article/details/53242126
 6. CGI glue between Nginx and Django

     !["各种cgi中间件速度对比图"](http://lazynight.me/wp-content/uploads/2012/09/uwsgi.jpg)
