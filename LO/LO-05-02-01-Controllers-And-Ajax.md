# Controllers-And-Ajax（控制器与异步请求）

## 简介

Laraoct中台实现了MVC设计模式。Controllers管理中台的页面，实现各种功能，如lists和forms。本文将详细介绍中台controllers的开发和行为(behaviors)匹配。

每个控制器的PHP文件都保存在插件的*controllers/*文件夹下。控制器的视图文件也保存在这个目录下。视图文件的后缀为`.htm`，这是Laraoct的独特后缀。视图的文件夹名称为控制器名称的小写字母。视图目录也保存着配置文件。示例如下：

```
plugins/
  acme/
    blog/
      controllers/
        users/                <=== 视图目录
          _partial.htm        <=== 视图部件
          config_form.yaml    <=== 配置文件
          index.htm           <=== 视图文件
        Users.php             <=== 控制器类
      Plugin.php
```

## 类定义

控制器类必须要继承`Lo\Classes\Controller`。与其他插件类相同，控制器也要正确书写插件的命名空间。最基本的控制器类如下：

```
namespace Acme\Blog\Controllers;

class Posts extends \Backend\Classes\Controller {

    public function index()    // <=== Action method
    {

    }
}
```

通常情况下，一个控制器仅处理一种类型（模型）的数据，像blog posts或categories。下文的描述，都是基于这个假设进行的。

## 控制器属性

中台的控制器拥有一系列的属性，用于配置页面展示效果和管理页面的安全性。

| 属性                 | 描述                                                   |
| -------------------- | ------------------------------------------------------ |
| $fatalError          | 保存action方法产生的致命错误，以便能够在前台展示       |
| $user                | 当前登陆用户的引用                                     |
| $suppressView        | 阻止视图显示。可以用在action方法或控制器的构造函数中   |
| $params              | 路由参数数组                                           |
| $action              | 当前请求的action方法名称                               |
| $publicActions       | 不需要登陆即可访问的action方法                         |
| $requiredPermissions | 访问所需要权限数组。主要是超管后台使用，中台基本不用。 |
| $pageTitle           | 页面标题                                               |
| $bodyClass           | 页面body css类                                         |
| $guarded             | 不能够作为action的方法名数组                           |
| $layout              | 指定一个特殊的布局                                     |


## Actions，视图和路由

控制器的公开(public)方法，称作action（因此就不翻译成中文了）。action名称与对应的视图文件相同时，将直接渲染此同名视图文件。例如，*index*这个action方法，对应的视图名称为*index.htm*。

中台的视图文件的语法遵循PHP基本语法。


中台的页面的URL，与超管后台不同。超管后台的URL是由插件作者名，插件名，控制器名和action名组成的。而中台的URL由插件名，控制器名和action名组成。

```
[plugin name]/[controller name]/[action name]
```

上面的控制器将通过如下url访问：

```
http://example.com/blog/users/index
```

## 视图数据

开发者通过控制器的`$vars`给视图传递数据。

```
$this->vars['myVariable'] = 'value';
```

通过`$vars`变量传递到视图的数据，可以直接在视图中直接使用。

```
<p>The variable value is <?= $myVariable ?></p>
```

## 使用AJAX处理器

Laraoct中台集成了AJAX库。中台的页面可直接使用AJAX库。

### 中台AJAX处理器

中台的AJAX处理器可以在控制器中定义，也可以在widgets中定义。在控制器中定义的话，方法必须为public，页面方法的名称必须以 **on** 开头，如 **onCreatedTemplate**, **onGetTemplateList**等。

AJAX处理器可以返回数组、抛出异常或页面跳转。还可以通过`$this->vars`设置参数，并且将控制器`makePartial`方法渲染的部件当作请求的响应数据，直接返回。

```
public function onOpenTemplate()
{
    if (Request::input('someVar') != 'someValue') {
        throw new ApplicationException('Invalid value');
    }

    $this->vars['foo'] = 'bar';

    return [
        'partialContents' => $this->makePartial('some-partial')
    ];
}
```

### 触发AJAX请求

可以通过两种方式触发AJAX请求。一种是通过标签data属性，一种是通过js api。例如：

```
<button
    type="button"
    data-request="onDoSomething"
    class="btn btn-default">
    Do something
</button>
```

具体详情，请参见AJAX部分内容