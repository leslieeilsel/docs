# LCG - Linux Configuration Guide - Disk
# 挂载数据盘
## 查看
* 查看磁盘信息  
	`df –h`  
	注：df仅能查看已分区和格式化的磁盘
* 查看分区  
	`fdisk -l`

## 分区
* 进行分区  
	`fdisk /dev/vdb`  
	根据提示，依次输入“n”，“p”“1”，两次回车，“wq”

* 格式化分区  
	`mkfs.ext3 /dev/vdb1`  

* 添加分区信息
	编辑`/etc/fstab`并添加如下内容：  
	`/dev/vdb1  /data ext3    defaults    0  0`
/dev/vdb1               /data                   ext3    defaults        0 0
* 挂载新分区  
	确认挂载路径`/data`已存在的条件下执行下列命令：
	`mount -a`
