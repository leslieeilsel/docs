# window下安装nvm、node.js、npm的步骤

## nvm安装

* 首先确保你的网络畅通，下载nvm包，地址：``https://github.com/coreybutler/nvm-windows/releases``;选择第一种需要自己配置，第二种不需要配置直接使用；

* 第一种自己配置:

1. nvm-noinstall.zip下载完成后解压到一个地方，比如C:\Users\Think\AppData\Roaming\nvm

2. 双击install.cmd打开文件，复制当前nvm的路径粘贴到``Enter the absolute path where the zip file is extracted/copied to:``**C:\Users\Think\AppData\Roaming\nvm**然后会生成一个settings.txt的文本文件只需要更改
`` root：C:\Users\Think\AppData\Roaming\nvm（nvm安装的位置）``
`` path：C:\Program Files\nodejs（快捷方式的位置）``

3. 配置环境变量：

    因为刚才点了install.cmd，那么在环境变量中就会生成两个变量： `` NVM_HOME 和 NVM_SYMLINK ``
    
    修改这两个变量的值为：``C:\Users\Think\AppData\Roaming\nvm 和 C:\Program Files\nodejs``

    Path的最前面输入``;%NVM_HOME%;%NVM_SYMLINK%;``

4. 打开cmd窗口输入命令:``nvm -v``我们就可以看到当前nvm的版本信息接下来就可以安装我们的node.js了。

* 第二种安装根据提示下一步操作即可成功

## node.js安装,npm资源优化以及版本切换


### node.js安装


nvm安装成功后我们就可以安装node.js了,但是会发现速度还是很慢，因为现在我们的下载地址还是外网，当网络环境比较差的时候可能就会下载失败；

解决方案：在配置nvm的时候有个settings的文件，在现有配置的后面添加两行代码

``node_mirror: https://npm.taobao.org/mirrors/node/``

``npm_mirror: https://npm.taobao.org/mirrors/npm/``

添加完成之后，保存好文件。关掉cmd，在重新打开cmd，输入：
````
nvm install [version] [32/64]
````
 即可安装对应的node版本了,下面把nvm操作node.js基础命令列出来：

* nvm arch ：查看当前系统的位数和当前nodejs的位数

* nvm install [version] [arch] ：安装制定版本的node 并且可以指定平台 version 版本号  arch 平台

* nvm list：查看已经安装的版本

* nvm list installed： 查看已经安装的版本

* nvm list available ：查看网络可以安装的版本

* nvm on ：打开nodejs版本控制

* nvm off  ：关闭nodejs版本控制

* nvm uninstall [version]：卸载制定的版本

* nvm use [version] [arch]:切换制定的node版本和位数

* nvm -v：查看当前nvm版本

### npm的优化

当我们配置安装好node.js之后，使用``npm install ``速度也会发现比较慢，所以同样的我们也需要使用镜像来加速依赖安装，设置很简单：
1. 先查看默认的regustry地址
````
npm config -g get registry
https://registry.npmjs.org/
````

2. 设置npm淘宝镜像
````
npm config -g set registry https://registry.npm.taobao.org
````