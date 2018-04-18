# Laraoct 开发手册

## 背景

Laraoct结合我们的实际应用场景，在October CMS的基础上做了大量改进。在LO3完成升级以后，这些改进也将逐步趋于完整和成熟。随着Laraoct改进内容的增多，新手上手越发困难。整理一个结构清晰，内容完整的开发文档的需要变得越来越强烈。

## 目录

* [安装及运行](./LO-01-Installation.md)
* [整体架构](./LO-02-Architecture-Concepts.md)
  * [生命周期](./LO-02-01-Lifecycle.md)
  * [Framework架构](./LO-02-02-Framework-Architecture.md)
  * [Module模块](./LO-02-03-Module.md)
  * [Plugins插件](./LO-02-04-Plugins.md)
  * [Laraoct特色](./LO-02-05-Laraoct-Feature.md)
* [Laraoct插件简介](./LO-03-Laraoct-Plugins.md)
  * [原生插件](./LO-03-01-OCT-Plugins.md)
    * [Builder](./LO-03-01-01-Builder.md)
    * User
    * Pages
  * [Laraoct基础插件](./LO-03-02-Base-Plugins.md)
    * API（API插件）
    * Department（组织机构插件）
    * Fuser（（中端）用户管理）
    * Information（基础资料）
    * Menu（菜单管理）
    * News（新闻管理）
    * Roles-And-Permission（角色和权限）
* [快速开发指南](./LO-04-Quick-Develop-Guide.md)
    * [插件快速开发指南](./LO-04-01-Quick-Plugin-Develop-Guide.md)
    * [轻量级前端快速开发指南](./LO-04-02-Quick-API-Frontend-Develop-Guide.md)
* [详细开发指南](./LO-05-Develop-Detail.md)
  * [插件开发指南](./LO-05-01-Plugin-Develop-Guide.md)
    * 插件注册
    * 插件版本管理
    * 配置
    * 本地化I18n
    * 扩展插件
  * [基于插件的中端开发详细指南](./LO-05-02-Mid-End-Develop-Guide.md)
    * [Controllers-And-Ajax（控制器与异步请求）](./LO-05-02-01-Controllers-And-Ajax.md)
    * [Views-And-Partials（视图和视图部件）](./LO-05-02-02-Views-And-Partials.md)
    * [Widgets（部件）](./LO-05-02-03-Widgets.md)
    * [Forms](./LO-05-02-04-Forms.md)
    * [Lists](./LO-05-02-05-Lists.md)
    * [Relations（关系）](./LO-05-02-06-Relations.md)
    * [Sorting-Records（列表排序）](./LO-05-02-07-Sorting-Records.md)
    * [Import-And-Exporting（导入导出）](./LO-05-02-08-Import-And-Exporting.md)
    * [Users-And-Permissions（用户和权限）](./LO-05-02-09-Users-And-Permissions.md)

## 文档说明

### 文件命名规则

* 总体命名规则，请参考https://coding.net/u/iba/p/tdoc/git/blob/master/H2/H2-HowToWriteTechDoc.md
* 层级关系，建议参考使用如下方式：

```
* LO-01-Installation.md
	- LO-01-01-Installation.md
	- LO-01-02-Configuration.md
		- LO-01-02-01-Database.md
		- Lo-01-02-02-App-config.md
```

* 如非必要，尽量不创建二级文件夹

## 写在最后

写文档也是写代码，都是不断迭代，不断进步的过程。

希望并鼓励甚至是要求每个人都参与进文档的更迭，一起探索，一起讨论，一起成长。

可能某一天，这个项目因你而伟大。