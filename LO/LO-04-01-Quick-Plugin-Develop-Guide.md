# 插件快速开发指南

## 示例目标

开发Example插件，并创建 TodoList 业务， 使得中端用户能够通过中端管理后台管理 TodoList内容。

## 前提条件

* 项目已经安装，并正确配置
* 开发环境正常，使用`http://localhost`能够访问项目

## 开发步骤

###  创建插件

创建插件有两种方式：

* 使用命令行；
* 使用Builder插件；

Builder插件内容，已经有详细文档描述。如果需要使用Builder插件创建插件，请参考相关文档。本文档仅介绍通过命令行创建插件。

创建插件命令：

```
php artisan create:plugin Iba.Example
```

运行命令后，将在`src/plugins/iba`目录下，创建`example`文件夹。

文件夹将包含以下内容：

```
src
....plugins
........iba
............example         //插件名称
................Plugin.php  //插件注册类
................version     //插件版本（数据迁移）管理
```

删除`Plugin.php` 的部分暂时不需要内容，最终文件内容如下：

```
<?php namespace Iba\Example;

use System\Classes\PluginBase;

/**
 * Example Plugin Information File
 */
class Plugin extends PluginBase
{
    /**
     * Returns information about this plugin.
     *
     * @return array
     */
    public function pluginDetails()
    {
        return [
            'name'        => 'Example',
            'description' => 'No description provided yet...',
            'author'      => 'Iba',
            'icon'        => 'icon-leaf'
        ];
    }
}
```



### 创建数据表

在`src/plugins/example/update`文件夹中，创建`build_todo_list_table.php`文件。文件内容如下：

```
<?php namespace Iba\Example\Updates;

use Schema;
use October\Rain\Database\Updates\Migration;

class CreateTodoListTable extends Migration
{
    public function up()
    {
        Schema::create('iba_example_todolist', function($table)
        {
            $table->engine = 'InnoDB';
            $table->increments('id');
            $table->string('name');
            $table->timestamps();
        });

    }
    
    public function down()
    {
        Schema::dropIfExists('iba_example_todolist');
    }
}
```

执行数据迁移：

```
php artisan october:up
```

### 创建model

运行命令，创建model。

```
php artisan create:model Iba.Example TodoList
```

修改`src/plugins/iba/example/models/TodoList.php`文件的`$table`属性为如下：

```
    /**
     * @var string The database table used by the model.
     */
    public $table = 'iba_example_todolist';
```

### 更新列表和表单

列表`src/plugins/iba/example/models/todolist/columns.yaml`

```
# ===================================
#  List Column Definitions
# ===================================

columns:
    id:
        label: ID
        searchable: true
    name:
        label: 任务名
        type: text
    created_at:
        label: 创建时间
        type: datetime
    updated_at:
        label: 更新时间
        type: datetime    
```

表单`src/plugins/iba/example/models/todolist/fields.yaml`

```
# ===================================
#  Form Field Definitions
# ===================================

fields:
    name: 
        lable: 任务名
        type: text
        span: full
```



### 创建Controller

运行命令，创建controller。

```
php artisan create:controller Iba.Example TodoList
```

控制器文件`src/plugins/iba/example/controllers/TodoList.php`内容如下：

```
<?php namespace Iba\Example\Controllers;

use Lo\Classes\Controller;

class TodoListController extends Controller
{
    public $implement = [
        'Lo.Behaviors.FormController',
        'Lo.Behaviors.ListController'
    ];

    public $formConfig = 'config_form.yaml';
    public $listConfig = 'config_list.yaml';

    public function __construct()
    {
        parent::__construct();

    }
```

### 配置业务插件

`config/cms.php`

```
    /*
    |--------------------------------------------------------------------------
    | Laraoct2 URI prefix
    |--------------------------------------------------------------------------
    |
    | Specifies the URL name used for accessing back-end pages.
    | For example: laraoct -> http://localhost/laraoct
    |
    */

    'loUri' => 'example',
```

### 配置业务权限

在后台用户->权限中，点击“自动添加所有权限”。将权限添加至表中。回到用户角色，将刚刚新建立的`iba_example_todolist`加入用户权限中。



最后，打开`https://localhost/example/todolist`即可访问内容。