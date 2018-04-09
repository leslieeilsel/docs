# LCG - Linux Configuration Guide - LAMP
# 安装配置LAMP环境

## 更新yum源
`sudo yum update`

## CentOS 6 安装配置
### Apache
* 安装Apache  
`sudo yum install httpd`
* 对配置文件`/etc/httpd/conf/httpd.conf`进行如下修改：  
```
DocumentRoot "/data/www/home"
<Directory "/data/www/home">
```
```
NameVirtualHost *:80
<VirtualHost *:80>
    	ServerAdmin webmaster@ibiart.com
    	DocumentRoot /data/www/home
    	ServerName ibiart.com
    	ErrorLog logs/home.ibiart.com-error_log
    	CustomLog logs/home.ibiart.com-access_log common
    	<Directory />
            Options FollowSymLinks
            AllowOverride None
    	</Directory>
    	<Directory /data/www/home/>
        	Options Indexes FollowSymLinks MultiViews
        	AllowOverride All
        	Order allow,deny
        	allow from all
    	</Directory>
	</VirtualHost>
```

* 启动  
`sudo chkconfig httpd on`


### PHP
* 安装PHP  
`sudo yum install php php-mysql php-pear php-mcrypt php-devel php-bcmath php-gd php-imap php-ldap php-pecl-igbinary php-pecl-memcached php-pecl-memcache libmemcached-last-libs php-pecl-msgpack php-soap php-xmlrpc php-mbstring php-pecl-mongodb`  

* 对/etc/php.ini进行如下修改  
`date.timezone =	Asia/Shanghai`  



* 重启服务  
`sudo service httpd restart`

### APC
* 安装依赖  
`yum install php-pear php-devel httpd-devel pcre-devel gcc make`

* 安装APC  
`pecl install apc`

* 启用  
`echo "extension=apc.so" > /etc/php.d/apc.ini`

* 配置  
`apc.shm_segments=1`
`apc.shm_size=128M`

* 安装管理页  
`cp /usr/share/pear/apc.php /var/www/html/apcadmin.php`

* 配置管理页  
`defaults('ADMIN_USERNAME','ibiart');       // Admin Username`
`defaults('ADMIN_PASSWORD','Set-Password-Here');  // Admin Password - CHANGE THIS TO ENABLE!!!`

### Varnish  
* 安装  
`rpm --nosignature -i https://repo.varnish-cache.org/redhat/varnish-4.0.el6.rpm`
`yum install varnish`

* 配置Varnish  
编辑`/etc/sysconfig/varnish`并将Varnish监听端口修改为80  
`VARNISH_LISTEN_PORT=80`

* 配置Apache  
将Apache中对应的监听端口改为8080  

* 额外配置  
如果要使用除8080以外的监听端口，可以修改`/etc/varnish/default.vcl`

* 启动  
`chkconfig varnish on`  
`service varnish start`

### 其他配置
* 防火墙
`sudo iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT`
`sudo service iptables save`
