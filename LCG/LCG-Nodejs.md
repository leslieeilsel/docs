# LCG - Linux Configuration Guide - node.js
# node.js配置及安装

## node.js安装
### 选择版本

* Node.js v4 LTS Argon  
`curl --silent --location https://rpm.nodesource.com/setup_4.x | bash -`  

* Node.js v5  
`curl --silent --location https://rpm.nodesource.com/setup_5.x | bash -`  


### 快速安装
* yum安装  
`sudo yum install -y nodejs`

* 编译工具（可选）  
`yum install -y gcc-c++ make`


## PM2
* npm安装  
`npm install pm2 -g`

## 配置node-gyp
* 安装 `npm install -g node-gyp`
* 确认 python 2.7 已安装（详细安装步骤参见[LDG Python](http://git.oschina.net/ibiart/Level0/wikis/LDG-Python)）
* 设置node-gyp的python路径 `node-gyp --python /usr/local/bin/python2.7`
* 设置npm的python路径 `npm config set python /usr/local/bin/python2.7`

## 安装gcc 4.9（支持C++11 Compiler）
* 下载DevTools3 repo `wget https://www.softwarecollections.org/repos/rhscl/devtoolset-3/epel-6-x86_64/noarch/rhscl-devtoolset-3-epel-6-x86_64-1-2.noarch.rpm`
* 安装repo `rpm -ivh rhscl-devtoolset-3-epel-6-x86_64-1-2.noarch.rpm`
* 安装gcc `yum install devtoolset-3-gcc devtoolset-3-gcc-c++ devtoolset-3-toolchain`
* 启用`scl enable devtoolset-3 bash`
* 检查gcc版本 `gcc -v`
* 在.bashrc中添加`source /opt/rh/devtoolset-3/enable`使设置永久生效
