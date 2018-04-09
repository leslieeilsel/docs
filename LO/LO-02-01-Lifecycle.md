# 生命周期

![生命周期](images\LO-02-lifecycle.png)

一图胜千言！

上图已经基本描述了Laraoct中，一个请求生命周期。

这个周期与Laravel基本相同，具体的描述，详见[Laravel Lifecycle](https://laravel.com/docs/5.6/lifecycle)的文档。

但如下几点，需要特别注意：

* `bootstrap/app.php`中注册的内核（HTTP/CONSOLE）和错误处理均为October CMS的内容；
* 在路由分发的过程中，October CMS在框架中启动时，就已经注册了系统路由。这跟Laravel完全自定义路由有比较大区别。系统路由，主要在如下文件中定义：
  * `src/modules/backend/routes.php`
  * `src/modules/cms/routes.php`
  * `src/modules/system/routes.php`
  * `src/modules/lo/routes.php`
* Laraoct系统仍然支持自定义路由，具体内容将会在插件开发部分介绍。