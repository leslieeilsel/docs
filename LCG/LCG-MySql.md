# LCG - Linux Configuration Guide - MySQL
# MySQL安装配置指引

### YUM源
* 在mysql community repository找到对应版本   
`https://dev.mysql.com/downloads/repo/yum/`
* 下载并安装  
`wget http://repo.mysql.com/mysql57-community-release-el7-11.noarch.rpm`  
`sudo rpm -ivh mysql57-community-release-el7-11.noarch.rpm`
`yum update`

### 安装
* 安装MySQL Server  
`sudo yum install mysql-server`  
`sudo service mysqld start`  
* 设置自动启动  
`sudo chkconfig --levels 235 mysqld on`  

### 配置
* 查看安装时生成的临时密码  
`sudo grep 'temporary password' /var/log/mysqld.log`

* 设置root密码，第一次提示输入当前root password时，直接回车即可。    
`sudo /usr/bin/mysql_secure_installation`  

* 测试安装
`mysqladmin -u root -p version`

* 设置mysql
`nano /etc/my.cnf`
```
[mysqld]
character_set_server = utf8
max_allowed_packet=16M
```

* 查看character-set  
`show variables like 'character%';`

### 修改MySQL默认路径  
* 创建新的数据目录
`mkdir /data/mysql/`
`chown mysql:mysql /data/mysql`

* 移动原始目录内容  
`mv /var/lib/mysql/* /data/mysql/`  

* 确认MySQL配置路径
默认配置路径在`/etc/my.cnf`或`/etc/mysql/my.cnf`  
可通过如下命令查看配置文件路径的加载顺序  
`mysqld --help --verbose | grep -A 1 "Default options"`  
屏幕输出类似下面内容  
`Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/etc/my.cnf ~/.my.cnf`

* 修改配置文件my.cnf  
把`datadir=/var/lib/mysql`修改为`datadir=/data/mysql`  
把`socket=/var/lib/mysql/mysql.sock`修改为`socket=/data/mysql/mysql.sock`  
加入client配置
```
[client]
port=3306
socket=/data/mysql/mysql.sock
```

* 检查SELinux  
`getenforce`  
如果返回`Permissive` 或 `Disabled` ，则可以跳过后续设置。  

* 添加context mapping  
`semanage fcontext -a -t mysqld_db_t "/data/mysql(/.*)?"`

* 使context mapping生效  
`restorecon -Rv /data/mysql`

* 重启MySQL  
`service mysqld start`

* 校验是否成功  
`mysql -u root -p`  
`show databases;`  

### 使用
* 使用root登录
`mysql -u root -p`

* 创建用户和数据库
`create database testdb;`
`create user 'testuser'@'localhost' identified by 'password';`
`grant all on testdb.* to 'testuser' identified by 'password';`
