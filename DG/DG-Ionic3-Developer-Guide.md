# lonic开发指南

## 运行环境要求

* node.js(8.11.2)

* java/sdk

* Android Studion/Genymontion

## 安装与运行


### node.js安装(v8.11.2 LTS)


版本不正确可能会报错误，或者npm依赖包未及时更新；node必须设置镜像，否则在下载npm包的时候会很慢;

node安装非常简单按照官方文档下一步就可以完成安装，此处需要注意的是,当你发现版本不对时，

Windows下选择覆盖版本的方法会更加方便解决问题；

### 全局安装ionic

* window下安装：``npm install -g cordova ionic``

* 其他系统下：``sudo npm install -g cordova ionic``

* 完成后，进入项目目录:``npm install``

* 最后浏览器中运行当前项目：``ionic serve(serve不是server)``

##### 完成后可能会出现问题：

* 网络问题(npm资源都是外国的所以网络会很慢甚至中断连接，cnpm虽然速度快但是也有诸多不便，不推荐使用)

* 版本问题：检查node版本、ionic版本(最新版本可能会有问题)、npm是否全局

### 项目打包运行--Ionic for Android

* ionic cordova platform add android

* ionic cordova emulate android(模拟器运行)

* ionic cordova run android（真机运行)

##### 在项目打包这里容易发生一些问题：
 
**注意看终端的报错地方逐一解决，一定要有耐心（如果身边有android的小伙伴你可以请他帮忙）**

**只有当你看到LAUNCH SUCCESS的时候说明你已经在Genymontion上运行成功了；**