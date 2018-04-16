# 插件开发指南

插件，官方介绍为`Plugins are the foundation for adding new features to the CMS by extending it.`，对于oct来说，插件是一种通过扩展方式向`CMS`添加新功能的方式。对于oct来说，插件主要服务于*CMS*，但是对于*Laraoct*来说，插件主要服务于中台，是通过扩展方式中台添加新功能的最有效方式。

对于oct，插件是主要的是components(组件)。因为component给前端提供定制化数据或功能。插件中controller大部分只是附属，原意是给oct的后台增加相应的数据管理功能。

但对于*Laraoct*而言，插件中的controller才是首位。因为*Laraoct*从核心module中，扩展出*中台*的概念，将管理后台功能前置（原有的后台管理则成为超级管理，权限最大）。

因此，本类文档更多的讲解插件关于controller部分。component(组件)的部分，将不做具体讲解，请参考oct官方文档。

对于插件的开发，带着以下几个问题阅读：

* 如何创建一个插件
* 如何将插件和系统关联（即所谓的注册）
* 插件版本如何管理？
* 插件能不能扩展，如何扩展？

## 插件注册

### 插件目录结构

创建一个插件之前，首要解决插件的“肉身”位置，插件文件放在什么位置，哪个目录。之前插件简介叫介绍过，Laraoct在psr0和psr4的基础上，建立了一套自身的autoload。因此，插件的“肉身”也有一个约定位置。

所有的插件，都位于*src/plugins*目录下，通常目录会包含如下内容：

```
plugins/
  acme/              <=== 插件作者名称
    blog/            <=== 插件命名空间
      classes/
      components/
      controllers/
      models/
      updates/
      ...
      routes.php     <=== 插件路由文件
      Plugin.php     <=== 插件注册文件
      plugin.yaml    <=== 插件注册配置文件
```

不是每个插件都必须包含所有文件，除了*Plugin.php*。

> 建议使用`rainlab.builder`插件生成和管理插件。通过这种自动化方式，可以在团队中保持一致风格。具体如何使用`Builder`插件，请参考相应文档。

### 插件命名空间

插件命名空间与文件位置关联，由插件作者名称、插件命名空间组成，如`插件作者名称\插件命名空间\具体类的文件夹名称`。
比如插件`src/plugins/acme/blog/controllers/TestController.php`的命名空间为`Acme\Blog\Controllers`。

Laraoct虽然没有对插件命名空间做大小写限制，但需要遵守文件夹名称小写，命名空间和类名驼峰，不使用下划线和中划线原则。

### 插件注册文件详解

每个插件都有且必须有一个`Plugin.php`文件，这个文件称为插件注册文件。插件注册文件记录了插件的相关信息，如名称、作者、描述等，也可以扩展对应的系统内容。

插件的相关信息，有两种配置方式。

一种是通过重写`Plugin.php`的方法来记录信息，具体详见[Supported methods](https://octobercms.com/docs/plugin/registration#registration-methods)。

另一种是通过`plugin.yaml`文件配置。示例如下：

```
plugin:
    name: 'ibiart.sas::lang.plugin.name'
    description: 'ibiart.sas::lang.plugin.description'
    author: ibiart
    icon: oc-icon-plane
    homepage: ''

```

> 强烈建议使用*Builder*插件生成和修改。

### 插件初始化

`Plugin.php`文件包含`boot`和`register`方法进行初始化，开发者可以利用这两个方法扩展模型、处理事件等。

当插件注册时，`register`将立即执行。而`boot`方法将在每个路由请求执行之前执行。所以，当依赖其他插件时，你应该使用`boot`方法。比如，扩展一个模型：

```
public function boot()
{
    User::extend(function($model) {
        $model->hasOne['author'] = ['Acme\Blog\Models\Author'];
    });
}
```

### 插件路由

插件直接支持自定义路由，只需要在插件名称下，添加`routes.php`文件。

```
//src/plugins/acme/blog/routes.php

Route::group(['prefix' => 'api_acme_blog'], function() {

    Route::get('cleanup_posts', function(){ return Posts::cleanUp(); });

});

```


### 插件依赖

一个插件依赖于另外一个插件，只需要在`Plugin.php`文件中，定义`$require`属性，属性值为依赖插件的名称。假设一个插件依赖*Acme.Blog*插件，可以这样写：

```
namespace Acme\Blog;

class Plugin extends \System\Classes\PluginBase
{
    /**
     * @var array Plugin dependencies
     */
    public $require = ['Acme.User'];

    [...]
}
```

插件依赖关系将会影响到插件的执行和更新顺序。

虽然系统支持定义复杂的插件依赖关系，但是为了避免循环依赖，应尽量避免定义多层依赖关系。


## 插件版本管理（Laraoct的数据迁移）

### 版本文件位置

在插件代码中，维护插件版本文件是一个非常好的开发习惯。版本管理文件不仅是用来声明版本变更记录，同时也是其他开发者（或使用者）进行插件数据迁移和数据填充的最好工具。

版本变更记录位于`/updates/version.yaml`，这个文件将记录所有的数据迁移和数据填充内容。示例如下：

```
plugins/
  author/
    myplugin/
      updates/                      <=== Updates 文件夹
        version.yaml                <=== 插件版本文件
        create_tables.php           <=== 数据迁移文件    
        seed_the_database.php       <=== 数据填充文件 
        create_another_table.php    <=== 数据迁移文件
```

### 版本升级触发条件

1. 管理员登陆后台Backend；
2. 使用后台Backend的更新插件功能；
3. 命令行输入`php artisan october:up`

### 插件依赖

插件可以按照一定的顺序进行更新，这种关系就是插件依赖。当插件存在依赖关系时，更新也按照依赖的顺序进行。插件之间只存在树型结构依赖，不存在相互环形依赖。

```
<?php namespace Acme\Blog;

class Plugin extends \System\Classes\PluginBase
{
    public $require = ['Acme.User'];
}
```
上述例子中，`Acme.User`更新之前，`Acme.Blog`不会进行更新。如果`Acme.User`不存在，系统将会报错。

### version.yaml详解

版本管理文件version.yaml按照版本顺序，每个版本都将包括版本变更记录和数据库脚本。关于数据库脚本，请参考数据库迁移文件指引。

示例如下：

```
1.0.1: First version
1.0.2: Second version
1.0.3: Third version
1.1.0: !!! Important update
1.1.1:
    - Update with a migration and seed
    - create_tables.php
    - seed_the_database.php
```


如上所述，每个版本文件包含两部分内容：

```
    版本号          版本备注
      ||               ||
    1.0.1:      First version
```

版本备注可以是字符串，也可以是数组。两种类型的数据，都可以描述版本变更说明和数据迁移（填充）两种类型。

如下示例展示一个既有版本备注，又有更新文件的内容。

```
1.1.1:
    - This update will execute the two scripts below.
    - some_upgrade_file.php
```

更新文件，文件名通常为sname_case，但是类别为CamelCase。例如，一个文件名为some_upgrade_file.php，其类名为`SomeUpgradeFile`。

> 强烈建议版本备注使用*一行备注，或者一行备注，一行数据迁移文件*。因为*Builder*仅能识别此种模式。而且，强烈建议使用*Builder*管理版本信息。


版本备注中，包含`!!!`的版本，为重大更新版本。这只会体现在后端Backend的版本升级和oct的market中。仅是一种标识，没有特殊功能。



## 插件的动态配置

## 本地化（i18n）

## 插件扩展