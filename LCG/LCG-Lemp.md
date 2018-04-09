# LCG - Linux Configuration Guide - LEMP
# 安装配置LEMP环境

## 更新yum源
* 添加Nginxyum源
创建repo文件`/etc/yum.repos.d/nginx.repo`，并复制如下内容：  
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
```

* `sudo yum update`

## CentOS 安装配置
### Nginx

* 安装Nginx  
`sudo yum install nginx`

* 启动Nginx  
CentOS6   
`sudo /etc/init.d/nginx start`  
CentOS7  
`sudo systemctl start nginx`

* 开机自动启动
CentOS6
`sudo chkconfig nginx on`  
CentOS7
`sudo systemctl enable nginx`

### PHP
* 安装PHP7.x  
`rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`  
`sudo yum install php70w php70w-fpm php70w-mysqlnd php70w-pear php70w-mcrypt php70w-devel php70w-bcmath php70w-gd php70w-imap php70w-ldap php70w-pecl-igbinary libmemcached php70w-soap php70w-xmlrpc php70w-mbstring php70w-pecl-mongodb`  

* 安装php5.x  
`sudo yum install php php-fpm php-mysql php-pear php-mcrypt php-devel php-bcmath php-gd php-imap php-ldap php-pecl-igbinary php-pecl-memcached php-pecl-memcache libmemcached-last-libs php-pecl-msgpack php-soap php-xmlrpc php-mbstring php-pecl-mongodb`

* 对/etc/php.ini进行如下修改  
`date.timezone =	Asia/Shanghai`  
`expose_php = Off`  
`max_input_vars = 1000000`  
`memory_limit = 2048M`
`post_max_size = 50M`
`upload_max_filesize = 50M`
`max_execution_time = 120`
`cgi.fix_pathinfo=0`  

* 重启服务  

CentOS7  
`sudo systemctl restart nginx`  
CentOS6  
`sudo service nginx restart`

### 配置Nginx  
* 编辑`/etc/nginx/nginx.conf`进行下列修改
```
worker_processes = 4
```

* 编辑`/etc/nginx/conf.d/default.conf`进行下列修改  
```
    server_name 127.0.0.1

    location / {
        root   /data/www/home;
        index index.php index.html index.htm;
    }

    location = /50x.html {
        root   /data/www/home;
    }

    location ~ \.php$ {
        root           /data/www/home;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
```

* 编辑`/etc/php-fpm.d/www.conf`并将user及group修改为nginx：  
```
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;	will be used.
; RPM: apache Choosed to be able to access some dir as httpd
user = nginx
; RPM: Keep a group allowed to write in log dir.
group = nginx
```


* 重启 php-fpm  
CentOS7  
`sudo systemctl restart php-fpm`  
CentOS6  
`sudo service php-fpm restart`

### 设置自动启动  
`sudo chkconfig --levels 235 nginx on`
`sudo chkconfig --levels 235 php-fpm on`

### SELinux
* 如果在确认www目录权限设置没有问题，但nginx仍提示403错误，可以检查如下设置  
`getenforce`
* 如果返回`Enforcing` ，则可以尝试将其设置为`Permissive`  
`setenforce Permissive`  
* 如果需要将其永久性设置为`Permissive`可编辑`/etc/sysconfig/selinux`
* 如果需要保持`Enforcing`，则可进行如下设置  
`chcon -Rt httpd_sys_content_t /path/to/www`
`setsebool -P httpd_can_network_connect on`
