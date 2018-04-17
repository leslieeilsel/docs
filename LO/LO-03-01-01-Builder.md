# Builder插件使用指南


## 简介

Builder插件（以下简称Builder)，是一个可视化开发工具。它通过代码生成和可视化操作，减少了开发插件的工作量。

使用Builder创建维护插件和手动创建维护，并没有什么不同。仅有部分手动创建的内容格式，Builder识别不出。对于Laraoct，提倡创建插件和数据迁移时，必须使用Builder。而对于其他功能，建议使用，如model的field和list配置的创建。但Laraoct扩展了部分特有的form field 或 list，这些内容，只能手动添加。


Builder能够做什么，请看下面列表：

* 创建插件，自动生成插件所必须的文件夹和文件。
* 数据迁移，包括创建数据表，更改数据表等。
* 创建Model类
* 创建forms匹配内容
* 创建list匹配内容
* 创建用户权限（主要指超管后台的权限）
* 创建后台导航
* 创建Controller，自动创建Backend的controller。但通过手动修改，可用于中台控制器。
* 管理插件版本和更新
* 提供全局组件，提供数据给前端用户使用。


## 使用指南

Builder提供了[官方视频](https://vimeo.com/154415433)，有兴趣可以前往了解。

使用Builder创建插件的一般流程。

```
初始化插件
    ||
创建数据表
    ||
创建model
    ||
创建form和list
    ||
创建controller
    ||
完成

```


### 创建并初始化插件

进入后台，打开Builder页面，从左侧边栏中点击小箭头按钮，打开插件列表。点击“创建插件”按钮，在弹出窗口中，填写插件名称、命名空间、插件icon。默认的作者名称和命名空间，可以到“后台=>设置=>Builder”中设置。

*注：当插件创建完成后，命名空间是不能在Builder里修改的。你需要手动修改。*

当Builder初化化插件后，会在`Plugins`文件夹下创建如下文件：

```
authornamespace
    pluginnamespace
       classes
       lang
           en
               lang.php
       updates
           version.yaml
       Plugin.php
       plugin.yaml
```

`plugin.yaml`文件包含插件的基本信息，如名称、描述、权限、后台菜单导航等。


### 创建数据表

通过Builder的Database（数据库）标签，可以对数据库的表定义进行可视化管理，可以对表结构进行CURD插件（注意：是表结构，不是表数据）。

点击“添加”按钮，即添加对应的数据表。界面非常简洁，不一一介绍。但需要注意以下几点：

* 点击保存按钮后，Builder会弹出窗口显示自动生成的代码，这个代码不能编辑，但是可以选择复制。当确认代码无误后，点击“Save & Apply”,Builder将会立即执行数据迁移，同时将自动生成代码保存至`updates`文件夹中，并更新`version.yaml`文件。

* 官方提醒，Builder虽然能够自动创建迁移文件代码，但在某些情况下，有可能会丢失字段或其他内容，所以需要认真核对自动生成的代码。确保数据迁移的正确性。

* Builder目前不支持索引管理，请手动修改迁移代码。

* Builder暂时不支持enum，请手动修改迁移代码。


### 创建模型

进入“Model”标签，点击添加按钮。按照界面提示操作，即可创建模型。

需要注意，Builder不支持模型的删除，需要手动删除对应的模型文件。

### 创建form和list

form和list都有对应的界面，一目了然，可自行体会。

### 创建Controller

通过界面，创建Controller，可选择对应的模型和菜单。操作也是比较简单，就不再详细描述。

如下事项需要注意：

* 因为Laraoct的菜单由另外的插件处理。所以菜单部分填写生成之后，需要手动修改代码。去除菜单部分内容
* 可以创建空Controller，也可以创建拥有Form/List/Reorder行为（Behavior）的Controller。但创建有行为的Controller时，先要创建对应的form.yaml或list.yaml。
* Controller创建之后，在Builder里面基本上不能再修改。如果需要修改，需要在代码中手动操作。

