# Views-And-Partials

**Views-And-Partials** 主要用来呈现中台前端页面显示效果，通过使用Views-And-Partials，可以方便快捷的构建一套组件化中台前端页面。

## Partials

Partials是views中最小的显示模块；我们可以把一个完整的html页面拆分成多个Partials，方便不同页面之间组合引用，示例如下：

在一个页面中我们经常需要引入css和js代码，一般情况下我们都会把整个项目所需要的所有css和js写在header标签中；而有了partials，我们只需在header中引用partials即可，而把引用的具体css和js文件交个具体partials处理

layouts.htm

```html
<?= $this->makeLayoutPartial('header') ?>
<body class="fixed-left <?= $this->bodyClass ?>">
```

_header.htm

```html
<!DOCTYPE html>
<html lang="<?= App::getLocale() ?>" class="no-js <?= $this->makeLayoutPartial('browser_detector') ?>">
<head>
<script src="<?= URL::asset('modules/lo/assets/js/jquery.min.js') ?>?v<?= $coreBuild ?>"></script>
 <link href="<?= URL::asset('modules/lo/assets/css/gantt.css') ?>?v<?= $coreBuild ?>" rel="stylesheet">
```

***注意：partials文件必须以_下划线开头***

上述操作即是一个对partials的简单操作，partials在构建页面过程中并不是必须的，但是通过partials我们可以方便的进行模块复用，从而提升我们的开发速度.

## Views

目前中台Views支持两种加载方式，第一种是在modules/lo/layouts下定义views文件，另一种是在/plugins/xxx/layouts下定义views文件，两种方式均可实现对views修改；如果以上两个文件都存在，那么plugins下views会优先加载

这里我们以一个plugins/下的views试图做为示例，以此来说明views用法

#### 目录结构

plugins

  .....layouts  views目录

​	.......default.htm 默认layouts

​	......._header.htm Partials

   .....assets 静态资源目录

​	.......css

​	.......js

​	.......image

#### 代码结构

在这里我们要先熟悉两个函数

```php+HTML
<?= URL::asset() ?>
<?= $this->makeLayoutPartial('') ?>
```

以上两个函数：

第一个可以方便的加载任何位置的assets静态资源文件;

第二个可以在layouts中方便加载当前layouts下的partials。

default.htm

```html
<?= $this->makeLayoutPartial('header') ?>
<body class="fixed-left <?= $this->bodyClass ?>">
<div id="wrapper"></div>
<div id="layout-flash-messages"><?= $this->makeLayoutPartial('flash_messages') ?></div>
<?= $this->makeLayoutPartial('script') ?>
</body>
</html>
```

上述就是我们一个简单的layouts/default.htm结构；通过将一个html拆分成多个partials，大幅提升了代码复用率

## Controller函数

views中所有数据都由访问的controller进行控制，同时我们在controller中可以定义一些属性，以实现不同的页面显示效果.

#### 引用不同layouts文件:

```php
public $layout = 'mycustomlayout';
```

通过上述方法，可以指定使用指定的layout渲染中台页面(需在layouts目录下定义对应layouts文件)

#### 指定bodycss

```php
public $bodyClass = 'compact-container';
```

这里只可指定如下三种模式:

- **compact-container** - 无填充效果
- **slim-container** - 左右无填充效果.
- **breadcrumb-flush** -与下面元素持平.

以上基本就是Views-And-Partials的操作，可参考[官方文档](https://octobercms.com/docs/backend/views-partials)