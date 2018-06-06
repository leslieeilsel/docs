## 简介

导入页面用户需要上传文件，并读取文件数据

导出页面用户需要读取数据库数据，生成文件下载

## 详细说明

#### 导入

在控制器里引用公共读取导入excel的Controller,添加配置导入文件

```
public $implement = [
	'Lo2\Behaviors\ListController',
	'Lo2\Behaviors\FormController',
	'Lo2\Behaviors\ImportExportController'
];
public $importExportConfig = 'config_import.yaml'; 
```

配置页面config_import.yaml

```
import:
    # Page title
    title: 导入数据

    # Import Model class
    modelClass: 读取数据添加数据库的类

    # Redirect when finished
    redirect: 执行完成跳转页面

    # Whether complex import
    complexImport: true
```

然后再model中将获取到的数据添加进数据表里



#### 导出

在控制器里添加初始化配置文件的方法

```
protected function reportHandler1(){
    $this->viewName = 'xxx';   配置视图名字
    $this->pageTitle = 'xxxx';  配置页面标题
    $this->initReportContainer('config_reports.yaml');  初始化报表显示容器
}
```

配置页面 config_reports.yaml

```
defaultWidgets: 默认报表插件
    complexSumary: 
        class: Lo2\ReportWidgets\SummaryTable 报表类
        sortOrder: 60 
        tableConfig: config_summary.yaml 表格配置
        configuration: 设置表格样式
            ocWidgetWidth: 10
```

表格配置 config_summary.yaml 

```
title: 标题
type: multiple
export: 是否导出  TRUE false
exportName: 表格名字
exportType: 文件类型 xlsx xls csv
exportOptions: 配置到样式
    mergeColumns: 合并列
        - A1【起始】:F1【结束】
    mergeRows: 合并行
        -
            columns:
                - A
            rows:
                -
                  - 3 起始
                  - 4 结束
    rowWidth: 列宽
        A: 10
    rowHeight: 行高
        1: 25
    cellStyle: 单元格样式
        A1:
            fontSize: 字体大小
            alignment: 水平样式 left center right
            valignment: 垂直样式 top center bottom
            border: 边框样式
                 top:
                    style: XX
                 right:
                    style:  XX
                 bottom:
                    style:  XX
                 left:
                    style:  XX
    columnFormat:  单元格格式化
        B5:F14: '#,##0.00' 

silentMode: true

header: 配置表头
    fields: 
        -
          - 月份,1,2
          - 体育彩票,5,1
        -
          - 乐透数字型
          - 竞猜型
          - 即开型
          - 视频型
          - 小计
body:表内容
    method:  调用方法 ，也可用sql
    value:
        method: model方法数据
filter: config_summary_filter.yaml  检索配置
```

检索配置 config_summary_filter.yaml

```
scopes:
    1:
        label:  检索名字
        type:  类型
```

如需其他参数，点击[请参考](https://laravel-excel.maatwebsite.nl/docs/2.1/export/sheets)



