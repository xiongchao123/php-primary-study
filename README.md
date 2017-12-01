# PHP学习之PHP7环境搭建
## Windows下环境搭建
### 一、下载Windows文件夹下phpstudy2016压缩包(一个不错的集成环境),解压安装。默认安装就可以。例如安装在(D:\phpStudy)
### 二、安装完成后打开phpstudy 默认是 非系统服务模式。可应用成系统服务(将webserver(Nginx/Apache/IIS等)和mysql注册为系统服务)。使用php7的话需要应用VC11以上的运行库。phpstudy会有提示。下载安装即可
### 配置环境
#### 1、为php设置环境变量(将php.exe所在目录D:\phpStudy\php\php-7.0.12-nts) 添加到path里即可
#### 2、打开cmd 输入 php -v 回车 如果输出php版本号 则设置环境变量成功
#### 3、配置Webserver (直接配置虚拟主机即可)
##### 1)  nginx 虚拟主机配置文件位于D:\phpStudy\nginx\conf\vhosts.conf  默认配置文件时该目录下的nginx.conf 打开文件编辑

server{

        listen       80;     ####监听端口
		server_name  127.0.0.1;   ####域名
        root   "D:/WWW";           ####项目路径
         location / {
            index  index.html index.htm index.php l.php;    ####项目默认访问文件 优先权从左向右递减
			#### try_files $uri $uri/ /index.php?$query_string;      #### Laravel 配置
           autoindex  on;                                          ####还没研究是干嘛的 可有可无
        }
       #### nginx关联php的配置 默认不用修改
        location ~ \.php(.*)$ {
            fastcgi_pass   127.0.0.1:9000;   #### 本项目监听的php-cgi 端口
            fastcgi_index  index.php;         
            fastcgi_split_path_info  ^((?U).+\.php)(/?.+)$;       
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PATH_INFO  $fastcgi_path_info;
            fastcgi_param  PATH_TRANSLATED  $document_root$fastcgi_path_info;
            include        fastcgi_params;
        }
}

##### 2)  apache 虚拟主机配置文件位于D:\phpStudy\Apache\conf\vhosts.conf  默认配置文件时该目录下的httpd.conf
##### 基本配置：
一、

<VirtualHost *:80>

　　ServerName www.test1.com

　　DocumentRoot /www/test1/

　　<Directory "/www/test1">

　　　　Options Indexes FollowSymLinks

　　　　AllowOverride None

　　　　Order allow,deny

　　　　Allow from all

　　</Directory>

</VirtualHost> 

二、基于端口

  修改配置文件(httpd.conf)
　　将原来的
　　 　Listen 80
      改为 
    　　Listen 80    Listen 8080

<VirtualHost 192.168.1.10:80>
    DocumentRoot /var/www/test1/
    ServerName www.test1.com
    </VirtualHost>

<VirtualHost 192.168.1.10:8080>
    DocumentRoot /var/www/test2
    ServerName www.test2.com
</VirtualHost>

#### 配置成功后重启下phpstudy  访问对应的域名:端口 即可访问站点

### 配置本地host(如果有需要)
#### 找到本地的hosts文件  如果是win10的话 需要先解锁hosts文件权限不然修改不了
#### 解析格式    如：127.0.0.1 www.weizhicheng.com   
####   127.0.0.1 www.weizhicheng.com  zhicheng.com xiongchao.com (这种方式也行。通过canme(别名方式)解析,三个域名都解析在了127.0.0.1)


## Linux下环境搭建(以Centos6.x为例)
### 安装PHP
#### 一：检查当前是否有安装php
rpm -qa|grep php

如果有安装PHP，那么请先删除这些安装包：

 yum remove php*

### 2.安装php源

Centos 5 安装php源：

rpm -ivh http://mirror.webtatic.com/yum/el5/latest.rpm

CentOs 6 安装php源：

  rpm -ivh http://mirror.webtatic.com/yum/el6/latest.rpm

CentOs 7 安装php源和epel扩展源：

rpm -ivh https://mirror.webtatic.com/yum/el7/epel-release.rpm

rpm -ivh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

###3.现在开始安装php

安装php5.5的基本安装包：

 yum install php55w php55w-gd php55w-mbstring php55w-mysql php55w-fpm

安装php5.6的基本安装包：

 yum install php56w php55w-gd php56w-mbstring php56w-mysql php56w-fpm

安装php7.0的基本安装包：

  yum install php70w php70w-gd php70w-mbstring php70w-mysql php70w-fpm

  其他扩展可自行安装

删除PHP：
rpm -qa|grep php
删除依赖：
rpm -e php-pdo-5.1.6-27.el5_5.3

### nginx的yum在线安装

添加源

wget http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm


安装源库

chmod +x nginx-release-centos-6-0.el6.ngx.noarch.rpm
rpm -i nginx-release-centos-6-0.el6.ngx.noarch.rpm


安装nginx

yum -y install nginx


安装完成后的默认配置文件路径

默认nginx配置文件: /etc/nginx/nginx.conf 【nginx主要的配置文件】
默认nginx的ssl配置文件: /etc/nginx/conf.d/ssl.conf 【配置SSL证书的，也可以并入到nginx.conf文件里】
默认nginx的虚拟主机配置文件: /etc/nginx/conf.d/virtual.conf 【如同Apache的虚拟主机配置，也可以并入到nginx.conf文件里】
默认的web_root文件夹路径: /usr/share/nginx/html 【web目录夹，放置Magento主程序】


配置iptables

iptables -I INPUT 5 -p tcp --dport 80 -j ACCEPT
(这个步骤，我之前机器上已经装过apache所以就忽略了)


安装后的目录：
 
配置目录：
/etc/nginx/

项目目录：
/usr/share/nginx/html/


## 安装Composer

CentOS下Composer的安装和使用
1. 下载composer.phar
curl -sS https://getcomposer.org/installer | php

2.把composer.phar移动到环境下让其变成可执行
mv composer.phar /usr/local/bin/composer

3.测试
composer -V

中国全量镜像
composer config -g repo.packagist composer https://packagist.phpcomposer.com


### Nginx配置

vi /etc/nginx/conf.d/default

server {
    listen       80;
    server_name  10.207.21.113;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    root /var/www/html/etm-m/public ;
    index index.php index.html index.htm;

gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
#gzip_http_version 1.0;
gzip_comp_level 8;
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
gzip_vary off;
gzip_disable "MSIE [1-6]\.";

    location / {
       #root   /var/www/html/etm-m/public;
       #index  index.php index.html index.htm;
       try_files $uri $uri/ /index.php?$query_string ;    #### 和windows下一样 laravel的配置 
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
        #proxy_pass   http://127.0.0.1;
   #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
    #     root           /var/www/html/etm-m/public;
        fastcgi_pass   127.0.0.1:9000;
        try_files $uri /index.php=404 ;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root/$fastcgi_script_name;
        include        fastcgi_params;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

### Nginx 负载
部署负载

nginx:

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;


    upstream myServer{
        ip_hash ;
        server  10.207.21.111:80;
        server  10.207.21.113:80;
   }

    server{
        listen 80;
        server_name etm ;
        location / {
           proxy_pass  http://myServer ;
           proxy_set_header Host  $host ;
           proxy_set_header X-Real-IP  $remote_addr ;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for ;
       }
   }

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
~


负载的 conf

server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

## 安装Mysql
Step1: 检测系统是否自带安装mysql

 yum list installed | grep mysql

Step2: 删除系统自带的mysql及其依赖命令：

要删除干净mysql

 yum -y remove mysql-libs.x86_64

Step3: 给CentOS添加rpm源，并且选择较新的源命令：

 wget dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
 yum localinstall mysql-community-release-el6-5.noarch.rpm
 yum repolist all | grep mysql

 yum-config-manager --disable mysql55-community
 yum-config-manager --disable mysql56-community
 yum-config-manager --enable mysql57-community-dmr
 yum repolist enabled | grep mysql

Step4:安装mysql 服务器命令：

 yum install mysql-community-server

Step5: 启动mysql命令:

 service mysqld start

Step6: 查看mysql是否自启动,并且设置开启自启动命令:

 chkconfig --list | grep mysqld

 chkconfig mysqld on

Step7: mysql安全设置命令：

 mysql_secure_instalation



修改mysql初始密码：

IK290yd7Dx2

方法一：

 /etc/init.d/mysqld stop

 mysqld_safe --user=mysql --skip-grant-tables --skip-networking &

 mysql -u root mysql

mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';

mysql> FLUSH PRIVILEGES;

mysql> quit

注：5.7 版本的 Password 字段已经改名为 authentication_string; 所以密码语句需要修改为：

UPDATE user SET authentication_string=PASSWORD('new') where USER='root';

赋权限

ALTER USER 'root'@'localhost' IDENTIFIED BY '45UIkdW230..';

grant all privileges on *.*  to  'root'@'%'  identified by '45UIkdW230..'  with grant option;


修改mysql时间：
rm -rf  /etc/localtime
cp  /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

-select now() 





