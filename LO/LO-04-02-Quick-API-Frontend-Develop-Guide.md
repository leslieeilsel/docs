# 轻量级前端快速开发指南

按照实现类型，前端开发主要包含以下两种模式：

* 传统web页面。服务器直接返回HTML页面。*注：SSR（单页应用的服务端渲染）不在本文档讨论范围。*
* SPA，单页应用。

传统web应用，服务器渲染完整的HTML页面后，返回给客户端（浏览器）。客户端（浏览器）直接展示对应的内容。SAP（单页应用）通过REST API方式获取数据，由客户端（浏览器）渲染视图。

Laraoct支持两种形式的web页面应用开发。而且两种方式的实现都非常的简单、快速。

## 传统web页面

**套页面**，是很多人对于服务端渲染HTML的理解。抛开各种成见，这个词确实准确的描述了服务端渲染HTML的主要途径和功能。所以，并不需要带着有色的眼光看待这个词语。同样是做刀，有些人做出刀连肉都切不好，有些人做出的刀却削铁如泥。Laraoct提供的**套页面**工具，正是这种利刃。

下面以Laraoct开发简单的todoList页面为例，串联起theme、layout、page、component等概念。如果需要更多了解这些内容，请参考其他章节的描述。

### 快速创建前端首页

* 创建新主题

  * `src/themes`文件夹中，新建example文件夹。文件内容如下：

    ```
    themes
    ....example
    ........pages
    ........theme.yaml
    ```

  * `src/themes/theme.ymal`文件内容如下：

    ```
    name: 'Laraoct example'
    description: 'The Default Theme for example'
    author: ibiart
    homepage: 'http://www.ibiart.com/'
    code: laraoct
    ```


* 创建首页

  * 创建`src/themes/page/index.htm`文件。*注意是htm后缀，并非常html后缀*

  * 文件内容如下：

    ```
    title = "home"
    url = "/"
    is_hidden = 0
    ==
    <h1>hello world</h1>

    ```

* 浏览器中，输入`http://localhost`。即可访问首页。首页内容显示`hello world`。

### 解析`index.htm`文件

`index.htm`以两个等号（==）分割，分为上下两部分内容。上半部分内容可称作为代码标签，用于记录路由、php代码等信息。下半部分内容为HTML内容。

上半部分代码解析如下：

```
title = "home"             //页面标题，显示于超管后台CMS模块的页面列表中
url = "/"                  //自定义路由。
is_hidden = 0              //是否在前端显示。0-显示，1-隐藏
==
```



### 创建更多页面

最后，根据实际业务需要，创建更多的页面即可。

* `src/themes/page/news.htm`

  ```
  title = "news"
  url = "/news"
  is_hidden = 0
  ==
  <h1>news</h1>

  ```

* `src/themes/page/about.htm`

  ```
  title = "about"
  url = "/about"
  is_hidden = 0
  ==
  <h1>about</h1>

  ```

* ...

为实现代码的封装和可重用性，Laraoct还提供了components功能。利用components提取通用性代码，或实现与后台数据交互。具体内容，请参考文章components。

## SAP

SAP项目同样有两种实现方式。考虑前后端分离部署，Laraoct可作为Server端，给SAP提供API。如果不考虑前后端分离部署的情况。同样可以按照上文描述的方式创建Page。当然，只需要创建一个page即可。
以VUE的脚手架为例：

```
title = "home"
url = "/"
is_hidden = 0
==
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <title>vue-laraoct-starter</title>
</head>

<body>
  <div id="app"></div>
</body>

<script src="../assets/js/app.js"></script>

</html>
```


API则需要进行部分调整。因为Laraoct的API插件仅对模型做了部分封装，能够通过简单的配置获取所有模型的数据。但也仅此而已。如果需要更复杂的逻辑或功能，需要对api插件进行二次开发或扩展。

因此，本文仅简单介绍api插件的配置，实现简单的数据获取。

### 路由

api插件路由地址为`src/plugins/iba/api/routes.php`，开箱提供两种路由。一个是auth路由，一个是基础模型的resource路由。

主要的resource路由如下：

```
//需要登陆后的操作
Route::middleware('Tymon\JWTAuth\Middleware\GetUserFromToken')->group(function() {
    //基本业务逻辑的curd，因为dingo/api的resource路由不支持create和edit，所以需要单独写
    //单独写的目的是提前判断是否有相应权限
    Route::get('sas/create',['as' => 'api.sas.create','uses' => 'SasController@create']);
    Route::get('sas/{id}/edit',['as' => 'api.sas.edit','uses' => 'SasController@edit']);

    //upload
    Route::post('sas/upload',['as' => 'api.sas.upload', 'uses' => 'SasController@upload']);
    Route::post('sas/deleteUpload',['as' => 'api.sas.deleteUpload', 'uses' => 'SasController@deleteUpload']);

    Route::resource('sas','SasController');
});
```
根据实际业余需求，配置对应的路由。

### 配置可访问模型

参考`src/plugins/iba/api/repositories/SasRepository.php`，其中的`whiteList`属性，可配置可访问模型的白名单列表。此文件仅作为参考，尽量不直接用于实际项目代码中。

### 配置可访问模型关系模型

参考`src/plugins/iba/api/resources/BaseResource.php`，其中的`availableIncludes`属性，可配置关系模型。