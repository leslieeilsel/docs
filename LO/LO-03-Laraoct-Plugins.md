# Laraoct插件简介

## 插件简介

目前，Laraoct存在三种类型的插件：

- October CMS官方插件
- Laraoct基础插件
- 业务插件

官方插件主要包括如下内容：

- Builder，通过界面的方式，生成CURD的模型、控制器和视图等；
- User，中端用户管理插件；
- Pages，CMS扩展插件，用于编辑静态内容；

Laraoct基础插件

- API（API插件），封装一套支持无状态认证的rest api插件；
- Department（组织机构插件）
- Fuser（（中端）用户管理）
- Information（基础资料）
- Menu（菜单管理）
- News（新闻管理）
- Roles-And-Permission（角色和权限）

业务插件

业务插件的命名空间位于Laraoct基础插件的命名空间中，即iba。在Laraoct中，业务需求是以单独的一个插件来实现各种功能。可参考Laraoct前期实现的各个项目代码。

具体插件的内容，将在后面的文档中详细介绍。

## 插件安装说明

October CMS提供了丰富的插件生态，详情见 [plugins](https://octobercms.com/plugins)。大部分插件都是免费插件。理论上，October CMS生态提供的插件，Laraoct都可以自由安装、删除。但基于系统开发的稳定性和质量保证，Laraoct仅安装官方团队开发的插件。对于第三方插件，项目开发者可参考原插件功能，自行开发。