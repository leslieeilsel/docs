# LCG - Linux Configuration Guide - Yum
# Yum源配置指引

## EPEL
通过yum安装  
`yum install epel-release`

或手动获取rpm  
* 获取对应版本的rpm包，更多信息详见`http://fedoraproject.org/wiki/EPEL`  
`wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`
`wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm`

* 安装rpm  
`sudo rpm -Uvh epel-release-6*.rpm`

* 进入`/etc/yum.repos.d`目录确保`epel.repo`设置为`enabled=1`

## IUS
* 获取对应版本的rpm包  
`wget http://dl.iuscommunity.org/pub/ius/stable/CentOS/6/x86_64/ius-release-1.0-13.ius.centos6.noarch.rpm`
`wget http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-14.ius.centos7.noarch.rpm`
* 安装rpm  
`sudo rpm -Uvh ius-release*.rpm`

* 进入`/etc/yum.repos.d`目录确保`ius.repo`设置为`enabled=1`


## Remi
* 获取对应版本的rpm包  
`wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm`

* 安装rpm  
`sudo rpm -Uvh remi-release-6*.rpm`

* 进入`/etc/yum.repos.d`目录确保`remi.repo`设置为`enabled=1`

## 可选操作
* 移除已有的安装  
`sudo rpm -e epel-release-6-8`


* 清除缓存  
`yum clean all `
