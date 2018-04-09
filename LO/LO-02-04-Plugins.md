# Plugins 插件

插件是构建Laraoct生态体系的基础。理论上，通过插件可以扩展出所有的系统功能，足见插件的强大 。插件plugins与模块modules在文件结构，注册形式等有非常多类似的地方。模块modules是Laraoct系统运行的基础，如果没有模块，系统将无法运行。而插件，顾名思义，是可以随时增加删除的内容。有需要的时候，就添加某个插件。某个项目不需要时，就可以移除对应的插件。

## 插件应用场景

简单来说，插件可分为如下内容：

* 组件（components）

* 控制器（controllers）


插件可应用于中、后端管理系统，也可以应用于前端。应用于前端的内容，插件将于组件（components）的形式出现。应用于中、后端的内容，插件将通过controllers进行对应的路由控制。

## 插件生命周期

* 组件生命周期

组件的作用是在CMS页面中任意位置输出对应的HTML内容。因此组件的生命周期包含在CMS页面中，是CMS页面的一部分。如在页面中，设置rain.user的account组件，页面即可显示登陆或注册的表单，并且实现登陆或注册的全部功能。

* 控制器生命周期

控制器的生命周期，类似于固定规则路由。通过接管Backend的request，寻找对应路径的控制器，处理完request后，返回对应的response.

规则如下：

```
backend/[author name]/[plugin name]/[controller name]/[action name]
```

举例：

```
http://example.com/backend/acme/blog/users/index 
//以下url，可访问名称为acme.blog的plugin，以及acme\controllers\Users文件的index方法
```

