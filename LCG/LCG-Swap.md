# LCG - Linux Configuration Guide - Swap
# Swap配置指引

## 检查
* 检查swap  
`swapon -s`

* 检查文件系统  
`df -hal`

## 创建新Swap
* 创建swap文件  
	创建大小为2G的swap文件  
	`sudo dd if=/dev/zero of=/swapfile bs=1024 count=2048K`

* 创建swap分区  
	`sudo mkswap /swapfile`

* 激活swap文件  
	`sudo swapon /swapfile`

* 设置随系统启动  
	编辑`/etc/fstab`，加入下列内容：  
	`/swapfile swap swap defaults 0 0`

## 配置Swappiness
* 检查当前设置  
	`cat /proc/sys/vm/swappiness`  
	swappiness取值在0-100之间。设置值约接近100，系统将越频繁的使用swap。当设置值为0时，系统仅在必要时使用swap。

* 修改设置  
	`sysctl vm.swappiness=0`

* 设置随系统启动自动应用
	编辑文件`/etc/sysctl.conf`，查找并修改如下内容：  
	`vm.swappiness=0`
