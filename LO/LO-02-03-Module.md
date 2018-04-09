# Modules 模块

Mudules主要包含如下部分：

- System Module
- Backend Module
- CMS Module
- Lo Module


## 模块通用目录说明

```
modules
....modules name      模块名称
........assets        模块资源（css/js/partial）目录   
........behaviors     模块behaviors目录
........classes       模块通用代码(class)目录
........controllers   模块控制器
........models        模块模型
........database      模块数据迁移
........lang          模块i18n内容目录
....composer.json     模块composer
....routes.php        模块路由
....ServiceProvider.php  模块ServiceProvider
```

## 模块介绍

### System Module

System Module是Laraoct的核心模块，也是唯一通过配置文件`app.providers`注册的模块，是整个项目的启动模块。基于配置文件的模块注册方式，请参考Laravel文档[Registering Providers](https://laravel.com/docs/5.6/providers#registering-providers)。

除核心System Module外，其他的模块将由System Module进行加载注册。其他的模块配置在`config/cms.php`中。具体如下：

```
/*
|--------------------------------------------------------------------------
| Determines which modules to load
|--------------------------------------------------------------------------
|
| Specify which modules should be registered when using the application.
|
*/
'loadModules' => ['System', 'Backend', 'Lo', 'Cms'],
```

根据配置，System Module将分别加载每个module下面的如下文件：

* `routes.php` 每个module的路由
* `ServiceProvider.php` 每个module的Provider

最后，System Module将注册所有的插件。

### Backend Module

后台管理模块，提供基础的CURD界面，列表、表单界面，以及各种后台设置界面等。主要内容包括如下 ：

* Controllers-And-Ajax（控制器与异步请求）
* Views-And-Partials（视图和视图部件）
* Widgets（部件）
* Forms
* Lists
* Relations（关系）
* Sorting-Records（列表排序）
* Import-And-Exporting（导入导出）
* Users-And-Permissions（用户和权限）

这部分内容的，详见Backend(详细)开发指南

###CMS  Module

CMS是前端内容管理平台，配合plugins的components可实现各种数据展示和实现需求。CMS模块的主要功能，可参考[October CMS](https://octobercms.com/docs/cms/themes)文档。

### LO Module

LO Module是Backend Module的一个扩展，让系统能够支持中端用户登陆，以及各种自定义需求。对于中端用户的特殊扩展，如果不能通过plugins实现的，或者具备通用性的功能。都可以通过LO Module来实现。

## 如何快速创建一个新模块

System Module、Backend Module、CMS module都是October CMS核心系统。

原则上，不应该（不允许）直接修改这些代码。应该通过扩展的方式，重写原有的功能，以适应实际需求。而且，Laraoct提供了丰富的plugins，所有的功能扩展，都应该通过plugins来实现。

考虑到现实需求中，还是会出现plugins无法实现的效果，因此，以Lo Module为例，简单介绍一下快速创建一个新Module的过程。

* 配置`config/cms.php`文件的`loadModules`字段，添加需要增加的模块名称，名称为小写英文字母，如lo
* 在`src\module`文件夹中，建立模块名称命名的文件夹，如`src\module\lo`。
* 创建`src\module\lo\ServiceProvider.php`，注册对应模块
* 如果有需要，可以创建`routes.php`
* 创建其他内容