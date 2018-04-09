# Developer Guide - Homestead
# Homestead安装配置指南

## 安装
### 安装VirtualBox
* 在如下地址下载最新的VirtualBox安装程序并进行安装  
`https://www.virtualbox.org/wiki/Downloads`  

* 安装VirtualBox Extension Pack
下载与对应VirtualBox版本相同的VirtualBox Extension Pack，下载完毕后双击vbox-extpack文件按照提示安装。

### 安装Vagrant
在如下地址下载Vagrant安装程序并进行安装  
`https://www.vagrantup.com/downloads.html`

### 安装Homestead Vagrant Box
* 在如下地址下载Homestead Vagrant Box  
`http://`
* 手动安装Homestead Vagrant Box
`vagrant box add laravel/homestead [本地下载路径\virtualbox.box]`

### 安装Homestead
* 进入home目录`cd ~`
  * Linux／Mac: `cd ~`
  * Windows: `cd C:\Users\[当前用户名]`
* 克隆Homestead到本地`git clone https://github.com/laravel/homestead.git Homestead`  
* 进入Homestead目录`cd Homestead`
* 初始化并生成Homestead.yaml
  * Linux／Mac: `bash init.sh`
  * Windows: `init.bat`  

* 修改homestead.rb文件  
为了避免手工安装box后反复下载的提示，打开`Homestead/scripts/homestead.rb`，找到`config.vm.box_version = settings["version"] ||= ">= 1.0.0"`，将其修改为`config.vm.box_version = settings["version"] ||= ">= 0"`

## 配置
