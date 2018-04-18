# Users-And-Permissions（用户和权限）

## 简介

Laraoct中台的权限与超管后台的权限完全不同。超管后台的权限，仍然延续oct后台权限，具体参见(oct文档)[https://octobercms.com/docs/backend/users] 。Laraoct中台的权限管理，是基于数据库表和表字段的，能够拥有更细粒度权限控制。

## 权限说明

Q: Laraoct的权限能够控制什么？

A: Laraoct的权限可以控制数据库中，业务数据表的访问(CURD）权限。通过情况下，业务数据表指的是所有以`ibiart_`或`iba_`开头的数据库表。可以简单的理解为，将哪些用户能够访问哪张数据表的权限保存起来，在需要的地方进行鉴权即可。

*注：其中`ibiart_`开头的数据表，是LO2的数据前缀。`iba_`开头的数据表，为LO3的数据表前缀。*

Q: Laraoct权限的深度如何，能够对数据表控制到什么程度？

A: 共有三个层级的权限控制程度。第一个层级为表级权限，即谁能读、写、更改、删除哪张表。第二层为字段级，即规定谁能够对某张表的某个或某几个字段进行读、写、更改、删除。第三个层级为行级，即规定某张表的某行数据，谁可以进行CURD。这三个层级，一级比一级的粒度细，实际应用也非常方便。比如，通过简单配置，即可让某个用户不能访问某个控制器内容（表级），或让其不能查看某个字段（字段级），或让其只能查看自己生成的数据（行级）。

## 权限的代码解析

权限代码由三个文件组成：

```
src/modules/lo/classes/PermissionBuilder.php
src/modules/lo/classes/PermissionCache.php
src/modules/lo/classes/PermissionManager.php
```

其中：

* PermissionBuilder用于生成对应权限
* PermissionCache用于缓存权限
* PermissionManager用于实际鉴权

PermissionManager主要通过以下方法鉴权：

* havePermission，此方法鉴定当前用户是否具有某个权限，如果有返回true，否则false。并且将生成该用户的鉴权缓存，缓存时间默认为5分钟


其他更详细的用法，请仔细阅读具体代码。


## Form和List的鉴权代码解析

由上可知，鉴权主要基于`PermissionManager`，而form和list的鉴权，无非就是在控制器的form和list的behavior上，增加对应的鉴权内容而已。

具体代码，详见：

* `src/modules/lo/behaviors/ListController.php`
* `src/modules/lo/behaviors/FormController.php`


## 权限设置说明

权限设置在超管后台的User导航中。

通常设置权限的流程为：

* 自动添加对应权限；
* 编辑用户组，配置所需要权限
* 将用户添加进对应的应用组

如果业务表比较多，在IO或内存较差的电脑上，可能无法完全添加用户权限至分组，目前这是一个bug。可考虑通过其他电脑直接导入数据。或仅添加某个数据表进行测试，而不是完全添加所有权限。