# LCG - Linux Configuration Guide - FTP
# 安装配置FTP

## 更新yum源
### 在ALIYUN ECS实例中快速更新
* 下载如下脚本  
`wget http://oss.aliyuncs.com/aliyunecs/update_source.tgz`
* 解压  
`tar -zxvf update_source.tgz`
* 以root身份执行下列命令：  
`sudo bash update_source.sh`

### 在其他环境中更新
* ...  

## 安装配置
### CentOS安装配置
* 安装vsftp  
`yum install vsftpd -y`

* 确认nologin位置  
通常在`/usr/sbin/nologin`或者`/sbin/nologin`下。

* 创建账户  
`useradd -d /data/ftp -s /sbin/nologin ftpadmin`  
注：其中`/data/ftp`为ftp home  

* 设置账户密码  
`passwd ftpadmin`  

* 设置home目录权限  
`chown -R ftpadmin.ftpadmin /data/ftp`

* 编辑`/etc/vsftpd/vsftpd.conf`并修改如下内容：  
	* 禁止匿名： “anonymous_enable=NO”
	* 取消如下配置前的注释符号：  
	local_enable=YES  
	write_enable=YES  
	chroot_local_user=YES  

* 确认`/etc/shells`中包含nologin条目

* 启动vsftp  
`service vsftpd start`
