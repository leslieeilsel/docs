## Ionic篇

### 1、安装JDK1.8

java运行环境，安卓开发必须

### 2、安装Note.JS （必须是v8版本及以下）

#### 坑 1：
本机环境是v9版本，第一次运行项目时报module依赖包丢失，在手动npm安装四五个依赖包后察觉不对，回溯代码发现问题：
![输入图片说明](https://gitee.com/uploads/images/2018/0203/140206_bb37ff68_907939.png "屏幕截图.png")

#### 原因：
sas_ionic3项目的依赖包未更新到v9版本
#### 解决：
使用nvm控制node版本后再进行npm安装


以下在“终端”操作

### 3、全局安装cordova ionic 

```
$ sudo npm install -g cordova ionic
```
### 4、在浏览器中运行sas项目

```
$ ionic serve
```
成功～！

### 5、Android Studio/Genymontion 安装

属于另一个领域的知识，在这爬了很多的坑（很多心酸泪），不再赘述。
（篇幅太长，后续考虑补充）

[Android Studio 2.0 教程从入门到精通MAC版，安装篇](http://www.linuxidc.com/Linux/2016-07/133418.htm)

### 6、项目打包运行—-Ionic for Android

根据网上的教程，运行命令如下：
* `ionic platform add android`
* `ionic emulate android`(模拟器运行)
* `ionic run android`（真机运行)

但在实际操作中，发现会报以下提示：
![ionic cordova platform](https://gitee.com/uploads/images/2018/0203/144807_f9a311d6_907939.png "屏幕截图.png")

#### 坑 2:
很多出现的错误是由于教程都是16年以前ionic2的，而由于近两年ionic热度大减，在大版本更新后，教程质量数量出现锐减。

#### 原因：
运行命令为ionic2的，使用ionic3命令运行

#### 解决：
* `ionic cordova platform add android`
* `ionic cordova emulate android`(模拟器运行)
* `ionic cordova run android`（真机运行)

### 7、解决原生模拟器响应问题
![模拟器](https://gitee.com/uploads/images/2018/0203/145941_83496e42_907939.png "屏幕截图.png")

#### 坑 3：
原生模拟器运行项目后黑屏，未桥接应用。

#### 原因：
原生模拟器运行占用系统性能多，8gb的Mac时常出现卡死或无响应的情况

#### 解决：
使用前两个步骤中安装的Genymontion模拟器替代原生。


### 8、Genymontion模拟器运行

当我配置好所有设置并打开Genymontion，运行

```
$ ionic cordova emulate android
```
无反应，再通过关键词谷歌搜索后发现，当模拟器为Genymontion时，此时Adb devices应该识别为真机。此时，运行命令改为：

```
$ ionic cordova run android
```

一切正常

### over