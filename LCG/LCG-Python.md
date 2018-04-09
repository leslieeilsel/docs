# LCG - Linux Configuration Guide - Python
# Python安装及配置指引
## 安装
### 确保已安装依赖
* `yum install gcc zlib-devel bzip2-devel openssl openssl-devel xz-libs wget sqlite-devel｀

### 下载并编译
* 下载源文件 `wget https://www.python.org/ftp/python/2.7.12/Python-2.7.12.tgz`
* 解压 `tar xzf Python-2.7.12.tgz`
* `cd Python-2.7.12`
* `./configure --enable-loadable-sqlite-extensions`
* `make`
* `make altinstall`

### 检查Python版本
* `python2.7 -V`

### 更改默认python版本
* `alias python='/usr/local/bin/python2.7'`

### 安装setuptools以及pip
* `wget --no-check-certificate https://bootstrap.pypa.io/ez_setup.py`
* `python2.7 ez_setup.py`
* `easy_install-2.7 pip`

### 手动安装PIP
* `wget --no-check-certificate https://github.com/pypa/pip/archive/9.0.1.tar.gz`
* `tar zvxf pip-9.0.1.tar.gz`
* `cd pip-9.0.1`
* `python2.7 setup.py install`

### 安装virtualenv
* `pip2.7 install virtualenv -i https://pypi.douban.com/simple`

### PIP安装scrapy及其依赖
* `pip2.7 install lxml parsel w3lib twisted cryptography pyOpenSSL pypinyin pysqlite Scrapy -i https://pypi.douban.com/simple`
