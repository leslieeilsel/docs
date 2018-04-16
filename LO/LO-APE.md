# APE 多用户商城功能选型文档

## 开源电商系统收集

| 名称     | 类型  | 地址                       |
| -------- | ----- | -------------------------- |
| ecshop   | B2C   | http://www.ecshop.com/     |
| ecmall   | B2B2C | http://ecmall.shopex.cn/   |
| shopnc   | B2B2C | http://www.shopnc.net/     |
| opencart | B2C   | https://www.opencart.com/  |
| megento  | B2C   | https://magento.com/       |
| WSTMart  | B2B2C | http://www.wstmart.net/    |
| tpshop   | B2C   | http://www.tp-shop.cn      |
| iwebshop | B2B2C | http://www.aircheng.com/   |
| Niushop  | B2B2C | http://www.niushop.com.cn/ |

*注：部分源码已上传到PAD，请至PAD下载查看。另，不是每个源码都保证正常安装，仅供参考*

## B2B2C参考选型

ecshop和ecmall作为早期的伪开源电商系统，虽然说不思进取，但是代表了当下主流电商平台的基本功能，具有比较明显的基础参考意义。特别昌ecmall是基于B2B2C的多用户商城，基础功能设计，可以作为适当参考。

opencart和megento作为国外的电商系统，本地化确实做的不好。而且megento的功能非常强大，体积也非常强大。所以，暂时忽略这两个系统，不进行深入参考。有兴趣可自行了解，欢迎分享。

剩下的几个电商平台，热心网友制作下图的对比。显然，Niushop作为后起之秀，就功能上来说，确实有比较多的参考价值。![](./images\ape_source_code.jpg)

因此，将选取以下几个开源代码，作为参考：

* ecmall
* iwebshop
* niushop

## 开发分析

目前暂定的方案是基于`iba.shop`插件扩展多用户商城，因此需要注意如下几点：

* 增加店铺用户体系
* 商品、订单等数据表，增加店铺信息
* 增加商用（店铺）中心的功能内容

### 增加店铺用户体系

`Laraoct`拥有两套用户体系，一套前、中台用户使用，一套后台管理用户。在电商系统中，前端用户体系已经用于购物端口的用户。而通过中台管理店铺的商家，即不适合归入购物用户，又不适合分到后台管理用户中。因此，需要为管理店铺的商家，提供一套新的独立用户体系。

### 商品、订单等数据增加店铺信息

增加店铺后，对应的数据都需要增加店铺信息。

### 商家店铺管理的功能

![](.\images\shop_2.png)