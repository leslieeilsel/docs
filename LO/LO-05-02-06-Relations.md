# Relations使用文档

## 简介

Relations是October中定义数据关联关系的一种简单实现，使用Relations，只需简单几行代码，就可实现一对一，一对多，多对多的关联操作，方便我们加快开发进度。

## 基础使用

 示例中我们会使用一个多对多的关系做为示例，以演示Relations的基础使用 
 一个商品会对应多个颜色，而一个 颜色可能会对应多个商品信息；在这里我们先设计三张表，用来实现多对多的关联关系（这里我们只简要描述3张表有关联关系的部分，其它字段做省略处理）

### product（商品表）

| name | comments |
| :--- | :------: |
| id   |   主键   |
| name | 商品名称 |

### option（颜色表）

| name | comments |
| :--- | :------: |
| id   | 颜色主键 |
| name | 颜色名称 |

### product_option（商品颜色对应表）

| name       | comments |
| :--------- | :------: |
| product_id | 商品主键 |
| option_id  | 颜色主键 |

1. 首先我们在controller中引用RelationController

```php
public $implement = [ 'Lo.Behaviors.FormController', 'Lo.Behaviors.ListController', 'Lo.Behaviors.RelationController',];
public $formConfig = 'config_form.yaml';
public $listConfig = 'config_list.yaml';
public $relationConfig = 'config_relation.yaml';
```

2. 定义config_relation.yaml

```yaml
propertyOptions:
    label: iba.shop::lang.products.property_option
    view:
        list: $/iba/shop/models/productpropertyoptionpivot/columns.yaml
        showSearch: false
        toolbarButtons: add|remove
    manage:
        list: $/iba/shop/models/propertyoption/columns_pivot_product.yaml
        form: $/iba/shop/models/propertyoption/fields.yaml
    pivot:
        form: $/iba/shop/models/productpropertyoptionpivot/fields.yaml
```

说明：config_relation.yaml中字段定义详见[october文档](https://octobercms.com/docs/backend/relations#configuring-relation)；piovt为多对多关联中间表数据，通过该字段可以方便操作中间表，有关pivot详见[laravel文档](https://laravel-china.org/articles/3798/laravel-skills-pivot);

3. propertyOptions多对多关联关系定义

```php
class Product extends Model{
    public $belongsToMany = [
    'propertyOptions' => [
        'Iba\Shop\Models\PropertyOption',
        'table'    => 'iba_shop_products_options',
        'key'      => 'product_id',
        'otherKey' => 'option_id',
        'pivot' => ['title', 'description', 'price_difference_with_tax', 'image', 'extended_quantity'],
        'pivotModel' => 'Iba\Shop\Models\ProductPropertyOptionPivot',            
    ],        
];
}
```
这里定义了一个相当复杂的关联关系模型，首先在october中，多对多关系均可写在`$belongsToMany`这个变量中；而`$belongsToMany`为一个key=>value形式的数组，key为关联关系名称（需与config_relation.yaml中对应）；有关october数据表多对多关联关系，请见[database](https://octobercms.com/docs/database/relations#many-to-many)

4. 经过上述简单配置，我们就可以在表单中使用Relations了

在/models/product/fields.yaml中配置使用Relations

```yaml
propertyOptions:
    tab: iba.shop::lang.products.properties_tab
    type: partial
    path: ~/plugins/iba/shop/models/product/_property_options_relation.htm
```

在/models/product/_property_options_relation.htm定义如下

```php+html
<?= $this->relationRender('propertyOptions') ?>
```
5. 经过上述操作，我们已经可以在中台正常使用Relations；下面为注意事项

- config_relation.yaml定义的view,manage,pivot需定义对应的fields.yaml与cloumns.yaml文件;
- 这里采用的示例是一个相当复杂的多对多过关联关系，如有兴趣可阅读lo3 ape分支下/plugins/iba/shop/product的实现

## Relations扩展功能

在实现上述基础功能的同时，Relations同时支持一些扩展功能；要使用该扩展功能，只需在controller中定义下述方法:

- 扩展yaml配置文件
- 扩展Relations view
- 移除指定列
- 扩展Relations manage
- 扩展Relations pivot
- 同步manage与view

以上方法使用，详见官网[Extending relation behavior](https://octobercms.com/docs/backend/relations#extend-relation-config)