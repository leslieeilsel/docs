# 01-安装与运行

## 运行环境要求

* PHP version 7.0 or higher
  * PDO PHP Extension
  * cURL PHP Extension
  * OpenSSL PHP Extension
  * Mbstring PHP Library
  * ZipArchive PHP Library
  * GD PHP Library
* Mysql version 5.6 or higher. （推荐使用5.7）

## Laraoct目录简述

```
root
....src 源码
....script 脚本
....doc 文档
....resource 资源 
README.md 项目说明文档
```



## 安装运行步骤

开发（发布）环境搭建不属于本文档范畴，具体参考Develop Guide相关指引。

本文档将对如下事项进行假定：

* 代码已经克隆至本地，并放置到正确目录，假定应用目录为`/home/www/`；
* `php`命令能正常使用；
* `composer`命令能够正常使用；
* 使用浏览器，打开`http://localhost`访问应用；
* 数据库访问地址为`localhost`，端口号为`3306`，账号`lo_user`，密码`password`，数据库名称为`lo_db`。

具体安装步骤如下：

* 将src/env-example文件复制一份副本出来，并将其重命名为.env
  ```bash
  cp env-example .env
  ```

* 配置数据库

  * 在.env中修改数据库相关配置，配置内容如下：

	```php
    # Database configurations. For more detail please refer to config/database.php
	DB_CONNECTION=mysql
	DB_HOST=localhost
	DB_PORT=3306
	DB_DATABASE=lo_db
	DB_USERNAME=lo_user
	DB_PASSWORD=password
  ```

    ​


* 进入项目`src`目录，通过composer命令，安装项目依赖包

```bash
//通过composer安装项目依赖包，具体执行过程如下：
cd /home/www/src
composer install
```

*注：最后将可能执行失败，但不影响最终的运行结果。失败的原因请查看  ([issue 3419](https://github.com/octobercms/october/issues/3419))*

* 配置加密key

  * 执行命令，创建加密key。

    ```php
    php artisan key:generate --show
    //base64:v7izc56aZoHJys7W10rN6vWUE5fFEx5ghDv+6ckiQmM=
    ```

  * 将所有生成的加密key，添加至`.env`文件中，具体如下：

    ```php
    APP_KEY="base64:v7izc56aZoHJys7W10rN6vWUE5fFEx5ghDv+6ckiQmM="
    ```

    ​

*  初始化项目数据库

```bash
php artisan october:up 
```



至此，访问`http://localhost`将能够正常访问前端页面。通过`http://localhost/backend`即可访问后台管理。



## 注意事项

暂无