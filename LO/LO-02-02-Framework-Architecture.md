# Framework架构

## 简述

在Laravel、October CMS基础上，Laraoct主要由modules和plugins组成。Modules构成了Laraoct的骨架。Plugins可以理解为骨架上的枝叶。通过开发不同的plugins，可以扩展出实际使用的各种功能。

Mudules主要包含如下部分：

* System Module，October CMS的启动框架，注册Server Provider和各类Class Alias；并提供基础命令、twig标签、插件管理、资源管理等内容。
* Backend Module，Laraoct超管后台，提供后台管理界面，基础模块的CURL框架，提供基础的列表、表单等内容。
* CMS Module，前端页面管理，即基于主题、文件的CMS系统。通过结合plugins的components，形成一个通用的CMS页面内容管理模块。
* Lo Module，Laraoct中端核心框架，扩展了Backend Module的各种基础功能，并增加了更细致的权限管理等内容。

Plugins主要包含内置插件、LO基础插件，业务插件。

## 目录结构说明

框架的内容均在`src`文本夹中，文件夹的具体功能如下：

```
src 代码根目录
....bootstrap    应用启动内容
....config       所有配置文件，包含应用配置、数据库配置、缓存配置等
....modules      系统模块
........backend  backend模块
........cms      cms模块
........lo       laraoct模块
........system   system模块
....plugins      插件
........iba      laraoct插件
........rainlab  october cms插件
....storage      文件、缓存目录
....tests        测试文件目录
....themes       cms 主题目录
....vendor       composer 包管理目录
artisan          命令行入口
composer.json    composer文件
index.php        应用入口文件
server.php       内置服务测试入口
phpunit.xml      phpunit配置
```

*注意：Laraoct所有文件目录以小写主，通常不使用snake命名方式。如ClassName的文件夹应该命名为classname。在plugins时，会再次强调。*

## 命名空间和自动加载说明

通常情况下，自动加载都会遵循`psr0`或`psr4`标准。Laraoct对于所有引用的外部类库也遵循此标准。

但是对于内部的Modules和Plugins，Laraoct有一套自己的标准，实现了一套加载Modules和Plugins的加载器。具体加载器代码，可参考`\October\Rain\Foundation\Bootstrap\RegisterClassLoader`文件。

* Modules目录下，命名空间将省略Modules，直接以文件夹名为命名空间。如文件`modules/backend/class/FormBehavior.class`的命令空间为`Backend\Class`
* Plugins目录下，命名空间也省略Plugins，直接以文件夹名为命名空间。如文件`plugins/iba/news/controllers/Posts.php`的命名空间为`Iba\News\Controllers`
* 在自动加载过程中，文件夹均以小写英文字母命名，原则上不使用snake和camel方式



## 

