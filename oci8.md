# php-oci8
# PHP安装Oracle扩展
### 下载rpm 
    
# 1.rpm包安装

###下载下来安装包后，

# 安装命令
##### $rpm -Uvh oracle-instantclient12.1-basic-12.1.0.2.0-1.x86_64.rpm
##### $rpm -Uvh oracle-instantclient12.1-tools-12.1.0.2.0-1.x86_64.rpm

ubuntu安装rpm包会出现问题 
1。alien的转换过程近乎没有提示信息，十分不友好，要是有进度条啥的就更好了！

2。Ubuntu下的软件包括rpm和alien的帮助都很容易看明白，-i[Install]...啥的，这样比较易记住命令！

RPM包转换为deb包安装：（两种方法）

方法一：

1. 先安装 alien 和 fakeroot 这两个工具，其中前者可以将 rpm 包转换为 deb 包。安装命令为：

sudo apt-get install alien fakeroot

2. 将需要安装的 rpm 包下载备用，假设为 package.rpm。

3. 使用 alien 将 rpm 包转换为 deb 包：

fakeroot alien package.rpm

4. 一旦转换成功，我们可以即刻使用以下指令来安装：

sudo dpkg -i package.deb

方法二：

1.CODE:

sudo apt-get install rpm alien

2.CODE:

alien -d package.rpm

3.CODE:

sudo dpkg -i package.deb
安装root用户
sudo apt-get install root-system-bin


# 2.pecl安装oci8
##### $yum install -y php-pear
##### $pecl install oci8

# 3.php.ini中设置
##### vi /etc/php.ini
##### 添加 extension=oci8.so

# 4.设置连接
##### $setsebool -P httpd_can_network_connect on

# 5. Docker容器中安装 php oracle扩展
   1. 先安装 gcc-c++ 
   2. 安装php7.0
   3. 安装 php70w-pear
   4. 安装 oci8 ；

# 6.编写case

    $conn = oci_connect('db_name', 'db_pass', 'db_ip:1521/SID');
    if (!$conn) {
        $e = oci_error();
        print htmlentities($e['message']);
        exit;
    }else{
        echo "Connect Oracle Success !" ;
    }

   
# 7. Windows中安装 php oracle扩展
   1. 先clone下载 php7-oci8.zip
   2. 解压后，把 instantclient_12_1 拷贝到C:\下，Path环境变量中加入 c:\instantclient_12_1 ; 
   3. 打开php的扩展（php的版本要和你所下载的instantclient_12_1版本兼容 否则php oci8模块将不能正常加载）
      (PS:WebServer服务注册为系统服务)
   4. 运行 php-m 测试 ；

# 8. Laravel5 使用 oci8 扩展

#### 1. 安装依赖
Installation [Laravel 5.4]
$ composer require yajra/laravel-oci8:"5.4.*"

Installation [Laravel 5.3]
$ composer require yajra/laravel-oci8:"5.3.*"

Installation [Laravel 5.2]
$ composer require yajra/laravel-oci8:"5.2.*"

Installation [Laravel 5.1]
$ composer require yajra/laravel-oci8:"5.1.*"


#### 2. 在config下创建oracle.php
```<?php

return [

    'oracle' => [
        'driver' => 'oracle',
        'host' => '180.163.69.191',
        'port' => '1521',
        'database' => 'NEWSADMIN',
        'service_name' => 'EMBASEGA',
        'username' => 'khdatasyn_r',
        'password' => 'khEHLhF',
        'charset' => '',
        'prefix' => '',

    ]
];

```



#### 3. 创建Controller

```<?php


class OracleController extends Controller
{
    public function home(){
        // 查询所有
        $result = DB::connection('oracle')->table('NEWSADMIN.LICO_ES_JPSITUA')->get() ;

        //按条件查询
        //$result = DB::connection('oracle')->table('NEWSADMIN.LICO_ES_JPSITUA')->where("ECHKSTATE",">",1000)->get() ;

        //执行Select语句
       // $result = DB::connection('oracle')->select("SELECT COUNT(1) FROM NEWSADMIN.TRAD_TD_TDATE WHERE TDATE = to_char(SYSDATE,'yyyymmdd') AND TEXCH IN ('CNSESH', 'CNSESZ')");

        /*
         * Delete/Update/Insert
         */
        //$result = DB::connection('oracle')->insert('insert into users (id, name) values (?, ?)', [1, 'Dayle']);
        //$result = DB::connection('oracle')->update('update tables set votes = 100 where name = ?', ['John']);
        //$result = DB::connection('oracle')->delete('delete from tables');
        var_export($result);
    }

}

```






   
