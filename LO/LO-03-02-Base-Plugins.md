# Laraoct基础插件

经过多个项目的实践，Laraoct积累了部分插件。这些插件最终沉淀为Laraoct的基础插件内容。基础插件将赋于Laraoct许多开箱即用的能力。

下面以插件名称首字母为序，简单介绍Laraoct的基础插件。

## API（API插件）

API插件为Laraoct提供基础的REST API。API基于jwt Auth0进行认证，实现接口不依赖于session。同时，API插件还提供数据权限管理，让客户端可通过API接口获取不同model的数据。

API插件提供如下接口：

* Auth相关
  * 注册
  * 登陆
  * 刷新token
* 业务相关
  * 某个model的CURD
  * 上传

## Department（组织机构插件）

组织机构为Laraoct提供层级关系的组织机构数据。如最基本公司内部组织机构。组织机构开箱即用，可通过后台管理添加。

## Fuser（（中端）用户管理）

基于官方用户管理插件开发的插件。主要用于如下用途：

* 中端用户登陆功能和界面
* 中端用户注册功能和界面
* 中端用户找回密码功能和界面
* 中端用户登陆配置，如验证码、访客模式等

## Information（基础资料）

实际使用的表单中，存在非常多的下拉菜单，如性别、省市区县、系统自定义类别等。为方便此类下拉菜单的key和value的管理，开发Information（基础资料）插件。

下拉菜单的值，可直接在后台中创建或修改。

在使用时，只需要按照如下方式使用：

```
public function getDropdownOptions() {
    return \Iba\Information\Models\Category::getOptionsByName($optionName);
}
```

## Menu（菜单管理）

中端菜单管理插件，用于管理中端界面的菜单，可配置多种url。

## News（新闻管理）

移植官方的blog插件而成，增加文件上传水印、视频转码等内容，并且修复了部分bug。主要用于企业、政府类网站发布新闻动态或其他文章内容使用。

## Roles-And-Permission（角色和权限）

Laraoct基于RBAC设置权限，并且根据业务需求，将权限控制由表级细化行级，甚至是字段级别。为方便使用，此插件还提供自动新增权限功能。能够将新添加的数据表权限，自动添加进权限列表中。