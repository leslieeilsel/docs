## 简介

工具管理，主要由前台页面和控制器组成。

##### 示例目录：

```
- widgets
  	- form
  		- assets
  			- css
  			- js
  		- partials		前台页面
  			- _form.html
  	- form.php			控制器
```

## 详细介绍

要将变量传递给partials，可以将它们添加到`$vars`属性中。 

```
public function render()
{
    $this->addJs('js/form.js');	//加载js
    $this->addCss('css/form.css');//加载css
    $this->vars['var'] = 'value'; 
    return $this->makePartial('form'); 渲染模板页面
}
```

前端页面调用ajax处理程序的方法，名称要有On开头

```
data-request="<?= $this->getEventHandler('onPaginate') ?>"
```

插件可以通过重写[插件注册类中](http://octobercms.com/docs/plugin/registration#registration-file)的`registerReportWidgets`方法来注册报表小部件。该方法应该返回一个数组，其中包含键和窗口小部件标签中的窗口小部件类以及值中的上下文。例： 

```
public function registerReportWidgets()
{
    return [
        'RainLab\GoogleAnalytics\ReportWidgets\TrafficOverview' => [
            'label'   => 'Google Analytics traffic overview',
            'context' => 'dashboard'
        ],
        'RainLab\GoogleAnalytics\ReportWidgets\TrafficSources' => [
            'label'   => 'Google Analytics traffic sources',
            'context' => 'dashboard'
        ]
    ];
}
```

