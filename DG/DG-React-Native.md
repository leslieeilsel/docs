# Developer Guide - React-native

# React-native开发安装配置指南

## 环境配置

### 1、安装LTS的node版本

[https://nodejs.org/en/](http://) 

安装完node后建议设置npm镜像以加速后面的过程。不建议使用cnpm

```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```

#### 安装Yarn、React Native的命令行工具（react-native-cli）

Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载。React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。

```
npm install -g yarn react-native-cli
```

安装完yarn后同理也要设置镜像源：

```
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
```

### 2、配置java环境

#### 安装jdk

[http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://)

#### 配置环境变量

##### JAVA_HOME

JDK的安装路径，这个环境变量本身不存在，需要创建，创建完则可以利用%JAVA_HOME%作为统一引用路径，其值为：jdk在你电脑上的安装路径。
![输入图片说明](https://gitee.com/uploads/images/2018/0204/155307_69156edb_1320745.png "屏幕截图.png")

##### PATH

PATH属性已存在，可直接编辑。作用是用于配置路径，简化命令的输入，其值为：%JAVA_HOME%\bin
![输入图片说明](https://gitee.com/uploads/images/2018/0204/160006_7425e01f_1320745.png "屏幕截图.png")

##### 检查java环境

打开终端，输入java -version和javac

![输入图片说明](https://gitee.com/uploads/images/2018/0204/162245_4064bd43_1320745.png "屏幕截图.png")

### 3、安装Android Studio

#### Android Studio

[http://www.android-studio.org/](http://)
下载完成之后安装
选择custom
![输入图片说明](https://gitee.com/uploads/images/2018/0204/180210_2cd07041_1320745.png "屏幕截图.png")

勾选Android Virtual Device
![输入图片说明](https://gitee.com/uploads/images/2018/0204/175705_32453288_1320745.png "屏幕截图.png")

- 安装完成后，在Android Studio的欢迎界面中选择Configure | SDK Manager


- 在SDK Platforms窗口中，选择Show Package Details，然后在Android 6.0 (Marshmallow)中勾选Google APIs、Android SDK Platform 23、Intel x86 Atom System Image、Intel x86 Atom_64 System Image以及Google APIs Intel x86 Atom_64 System Image。

![输入图片说明](https://gitee.com/uploads/images/2018/0204/185450_d5b03847_1320745.png "屏幕截图.png")

- 在SDK Tools窗口中，选择Show Package Details，然后在Android SDK Build Tools中勾选Android SDK Build-Tools 23.0.1（必须包含有这个版本。当然如果其他插件需要其他版本，你可以同时安装其他多个版本）。然后还要勾选最底部的Android Support Repository.

![输入图片说明](https://gitee.com/uploads/images/2018/0204/185636_7ddcc6e9_1320745.png "屏幕截图.png")

#### ANDROID_HOME环境变量

- 确保ANDROID_HOME环境变量正确地指向了你安装的Android SDK的路径

![输入图片说明](https://gitee.com/uploads/images/2018/0204/222347_d52fe184_1320745.png "屏幕截图.png")

## 项目结构

### 目录结构

- root
- android 各种配置文件
- ios 各种配置文件
- js 页面源码
- res 静态资源
- index.android.js  android入口文件
- index.ios.js  ios入口文件
- package.json  依赖包

### 项目运行

- clone项目到本地
- 执行npm install，安装依赖
- 执行adb devices查看连接的设备或模拟器
- 安装完毕，执行react-native run-android可在android模拟器或者真机运行代码

## 项目打包——Android

#### 生成一个签名密钥

- 使用keytool生成密钥. 在Window系统需要进去C:\Program Files\Java\jdkx.x.x_x\bin.目录才能运行keytool

```
$ keytool -genkey -v -keystore my-keystore-file.keystore -alias android/app/sas -keyalg RSA -keysize 2048 -validity 10000
```

- 这条命令会要求你输入密钥库（keystore）和对应密钥的密码，然后设置一些发行相关的信息。最后它会生成一个叫做sas.keystore的密钥库文件。

### 设置gradle变量

#### 全局

- 把sas.keystore文件放到你工程中的android/app文件夹下。（已经生成到目录中）
- 编辑~/.gradle/gradle.properties（没有这个文件你就创建一个），添加如下的代码
- 注意：~表示用户目录，比如windows上可能是C:\Users\用户名，而mac上可能是/Users/用户名。

```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```

上面的这些会作为全局的gradle变量，我们在后面的步骤中可以用来给应用签名。

#### 局部

- 在android/gradle.properties中添加上述代码

##### 注意把其中的****替换为相应密码

### 添加签名到项目的gradle配置文件

编辑你项目目录下的android/app/build.gradle，添加如下的签名配置

```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

### 生成发行APK包

只需在终端中运行以下命令：

```
$ cd android && ./gradlew assembleRelease
```

生成的APK文件位于android/app/build/outputs/apk/app-release.apk，它已经可以用来发布了。

### 打包没有bundle

```
React-native bundle --platform android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
```

参考自：[https://facebook.github.io/react-native/docs/signed-apk-android.html#docsNav](http://)

