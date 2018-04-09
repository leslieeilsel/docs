# WCG - Windows Configuration Guide - WAMP
# 安装配置WAMP环境

## Apache
### 下载安装
* 下载VC14 Redistributable Pack 32位版  
  `https://www.microsoft.com/en-us/download/details.aspx?id=48145`
* 下载Apache  
  从如下链接下载32位版的Apache  
  `https://www.apachelounge.com/download/VC14/binaries/httpd-2.4.20-win32-VC14.zip`  
  或选择其他版本 `https://www.apachelounge.com/download/`
* 将解压后内容放置于任意位置，例如`c:/Apache24`

### 配置
* 打开Apache工作目录下conf/httpd.conf并进行如下配置：  
  1.确认所有的相关配置为`c:/Apache24`  
  2.修改监听端口 Listen *:80  
  3.打开`LoadModule rewrite_module modules/mod_rewrite.so`  
  4.修改`DocumentRoot "c:/Apache24/htdocs"`为`DocumentRoot "d:/www/default"`  
  5.修改`<Directory "c:/Apache24/htdocs">`为`<Directory "d:/www/default">`  

### 验证  
* 打开命令行窗口，进入Apache工作目录下的bin，如`c:/Apache24/bin`
* 执行`httpd -t`，如果没有提示错误，则表示配置正确。

### 以windows service运行Apache
* 打开命令行窗口，进入Apache工作目录下的bin，如`c:/Apache24/bin`
* 执行`httpd -k install`  

## PHP
### 下载安装
* 下载VC11 Redistributable Pack 32位版  
  `https://www.microsoft.com/zh-CN/download/details.aspx?id=30679`  
* 从如下链接下载Thread Safe32位版   
  `http://windows.php.net/downloads/releases/php-5.6.21-Win32-VC11-x86.zip`  
  或选择其他版本 `http://windows.php.net/download/`
* 将解压后内容放置于任意位置，例如`c:/php`  

### 配置
* 打开PHP工作目录下php.ini并进行如下配置：
  1.取消`extension_dir = "ext"`前的注释
  2.取消`extension=php_curl.dll`前的注释  
  3.取消`extension=php_gd2.dll`前的注释  
  4.取消`extension=php_mbstring.dll`前的注释  
  5.取消`extension=php_mysql.dll`前的注释  
  6.取消`extension=php_mysqli.dll`前的注释  
  7.取消`extension=php_xmlrpc.dll`前的注释  
  8.取消`extension=php_soap.dll`前的注释  
  9.取消`extension=php_sockets.dll`前的注释  
  10.取消`extension=php_xsl.dll`前的注释  
  如上扩展设置可以满足常见的情况，如有特殊需要，可根据情况打开其他扩展。  

* 添加环境变量  
  将`;c:/php`添加至系统环境变量中path的末尾  
  ![输入图片说明](http://git.oschina.net/uploads/images/2016/0520/134554_38d058f7_86643.png "在这里输入图片标题")

* 打开Apache工作目录下conf/httpd.conf并进行如下配置：
  1.添加index.php为默认页`DirectoryIndex index.php index.html`  
  2.在文件尾部添加如下内容，将php配置为Apache module  
```
# PHP5 module
LoadModule php5_module "c:/php/php5apache2_4.dll"
AddType application/x-httpd-php .php
PHPIniDir "C:/php"
```

### 验证  
* 打开命令行窗口，进入Apache工作目录下的bin，如`c:/Apache24/bin`
* 执行`httpd -t`，如果没有提示错误，则表示配置正确。
* 添加info.php文件并填充如下内容`<?php phpinfo(); ?>`

## 单个Apache实例运行多个PHP版本
### 下载安装
* 从如下链接下载Non-Thread Safe (NTS) 32位版   
  `http://windows.php.net/download/`  
  如果需要较早的版本，可在这里找到
  `http://windows.php.net/downloads/releases/archives/`
* 将解压后内容放置于任意位置，例如`c:/php56` `c:/php70`

### 配置PHP
* 打开PHP工作目录下php.ini并进行如下配置：
  1.取消`extension_dir = "ext"`前的注释
  2.取消`extension=php_curl.dll`前的注释  
  3.取消`extension=php_gd2.dll`前的注释  
  4.取消`extension=php_mbstring.dll`前的注释  
  5.取消`extension=php_mysql.dll`前的注释  
  6.取消`extension=php_mysqli.dll`前的注释  
  7.取消`extension=php_xmlrpc.dll`前的注释  
  8.取消`extension=php_soap.dll`前的注释  
  9.取消`extension=php_sockets.dll`前的注释  
  10.取消`extension=php_xsl.dll`前的注释  
  如上扩展设置可以满足常见的情况，如有特殊需要，可根据情况打开其他扩展。  

* 添加环境变量  
  将`;c:/php`添加至系统环境变量中path的末尾  
  ![输入图片说明](http://git.oschina.net/uploads/images/2016/0520/134554_38d058f7_86643.png "在这里输入图片标题")

### 配置Apache
* 确保apache extension_dir目录下包含如下扩展，并在httpd.conf中启用。
  `# LoadModule include_module modules/mod_include.so`
  `# LoadModule fcgid_module modules/mod_fcgid.so`
  `# LoadModule vhost_alias_module modules/mod_vhost_alias.so`、

* 开启vhost并配置多个php版本：在`httpd.conf`中启用`# Include conf/extra/httpd-vhosts.conf`，然后打开`conf\extra\httpd-vhosts.conf `进行如下配置  

```
<VirtualHost *:8000>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "d:/ides/web"
    ServerName serverip:8000
    ServerAlias www.dummy-host.example.com
    ErrorLog "logs/dummy-host.example.com-error.log"
    CustomLog "logs/dummy-host.example.com-access.log" common

	<Directory "d:/www/web56">
		<Files ~ "\.php$">
			AddHandler fcgid-script .php
			FcgidWrapper "c:/php56/php-cgi.exe" .php
			Options +ExecCGI
			order allow,deny
			allow from all
			deny from none
		</Files> 		
	</Directory>
</VirtualHost>

<VirtualHost *:9000>
    ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "d:/www/access_control"
    ServerName serverip:9000
    ErrorLog "logs/dummy-host2.example.com-error.log"
    CustomLog "logs/dummy-host2.example.com-access.log" common

	<Directory "d:/www/web70">
		<Files ~ "\.php$">
			AddHandler fcgid-script .php
			FcgidWrapper "c:/php70/php-cgi.exe" .php
			Options +ExecCGI
			order allow,deny
			allow from all
			deny from none
		</Files>
	</Directory>
</VirtualHost>
```

* 可选的配置步骤：  
设置默认PHP版本：在`httpd.conf`中启用`# Include conf/extra/httpd-default.conf`，然后打开`conf\extra\httpd-default.conf`，并在末尾添加如下内容  
```
[nginx]
FcgidInitialEnv PATH "c:/php70;C:/WINDOWS/system32;C:/WINDOWS;C:/WINDOWS/System32/Wbem;"
FcgidInitialEnv SystemRoot "C:/Windows"
FcgidInitialEnv SystemDrive "C:"
FcgidInitialEnv TEMP "C:/WINDOWS/Temp"
FcgidInitialEnv TMP "C:/WINDOWS/Temp"
FcgidInitialEnv windir "C:/WINDOWS"
FcgidIOTimeout 64
FcgidConnectTimeout 16
FcgidMaxRequestsPerProcess 1000
FcgidMaxProcesses 50
FcgidMaxRequestLen 8131072
# Location of php.ini
FcgidInitialEnv PHPRC "c:/php70"
FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
<Files ~ "\.php$">
  AddHandler fcgid-script .php
  FcgidWrapper "c:/php70/php-cgi.exe" .php
  Options +ExecCGI
  order allow,deny
  allow from all
  deny from none
</Files>
```
 **注：在部分环境下开启httpd-default.conf，可能会导致所有版本的php均从默认路径加载php.ini。**
