## 



【差异点工时评估】
1、接口传参：cookie传参，接口入参差异处理
2、接口返回值：
	1）返回结构差异：涉及较大的前端代码结构逻辑变化
	2）字段缺失：原接口字段新接口没有
	3）字段新增：新接口新增字段所需的页面展示开发
3、数据驱动模式：除页面样式外，其余页面结构逻辑相当于重做
    重新做各个模块具体工时评估如下：

1、app的功能清单不详，可能越做越多，有待确认项目

2、接口89几个接口，一个接口联调一天也得89天

3、工具化实现，未来的多端场景

-----

**待确定：埋点是否出新方案**

后端：适配各端，屏蔽sku

  黄金流程工时估算：105天
        1）商祥：30（含26个组件） 30  --> 24天  **PLUS 价/重逢价/学生价等价格优先级**
            1）快捷登录 -1
            2）商详-主图楼层(apollo-item-bpMainImage) 、商详-广告图(apollo-item-bpPromot)  -1d
            3）商详-价格楼层(apollo-item-bpJPrice) 、商详-名称楼层(apollo-item-bpName)   -1d
            4）商详-优惠券楼层(apollo-item-prodFavorableInfo) 促销 --2d	 **比较复杂**
            5）商详-规格楼层(apollo-item-bpChoice)   --2d     	**比较复杂**
            6）商详-下单按钮楼层(apollo-item-footBtn)  -2d 	**比较复杂**
            7）商详-地址楼层(apollo-item-bpAddr)、重量楼层(apollo-item-bpWeight)**???** -2d **地址楼层比较复杂**
            8）商详-推荐楼层(apollo-item-bpRecommend)  -2d
            9）商详-评价楼层(apollo-item-bpEvaluate)  --3d 
            10）商详-预约-彩带楼层(apollo-item-bpYuyueBanner)、商详-预约楼层(apollo-item-bpYuyue) -1d
            11）商详-秒杀-彩带(apollo-item-bpSecBanner)、商详-闪购(apollo-item-bpFlashBanner) -1d
            12）商详-国际-税费(apollo-item-bpTax.wxml)、商详-预约文案(apollo-item-adWordText)、商详-店铺楼层(apollo-item-bpShop) -1d				 **bdapp没有**  
            13）商详-预售(apollo-item-bpYushou)、商详-预售彩带(apollo-item-bpYushouBanner) -1d
            14）商详-LOC(apollo-item-locSelect)、商详-LOC-下单流程(apollo-item-locStep) -2d **比较复杂.极速M目前没有**
            15）商详-重量(apollo-item-bpWeight)、商详-服务楼层(apollo-item-bpServe) -1d
            16）商详-详情介绍图(apollo-item-bpDetails) -1d

​			17)   遗漏: 京喜拼购楼层（apollo-item-bpMianJXPinGouPrice）,拼购楼层（apollo-item-bpMianPinGouPrice）,分享	

​        2）购物车：26天 --> 22天
​            1）商品主信息(2d)
​            2）促销信息、赠品/换购商品(2d)
​            3）选择/取消商品：选单商品、按店铺选、全选/取消；删除商品/赠品(2d)
​            4）所有弹框：优惠券(2d) 、其他（2d） 
​            5）换购(3d)  -->2d
​            6）领赠品(2d) -->1d
​            7）切换促销(2d) -->1d
​            8）双价格切换(2d) 
​            9）切换全球购/非全球购(1d)
​            10）小商详切换规格(3d)
​            11）快速清理(3d) -->2d

​        3）结算：28天。-->25天
​            1）主商品信息(4d)
​            2）二级页-配送(3d) -->2d
​            3）地址列表(2d)  
​            4）新建/编辑地址(3d) -->2d
​            5）弹框 - 红包(3d) 
​            6）京豆(3d) -->1d
​            7）优惠券(3d)
​            8）发票(4d) -->2d
​            9）支付联调(3d)
​        4）订单：12天
​            1）订单列表页：4
​            2）订单详情页：4
​            3）物流跟踪页：2
​            4）评价中间页：1
​            5）客服H5: 0.5
​            6）评价H5: 0.5
​        5）个人中心：11天
​            1）我的优惠券：2
​            2）主页：2
​            3）反馈：1
​            4）收获地址：1
​            5）我的预约：2
​            6）登陆以及账号设置页面：3

【情况】
1、目前极速M站与极速小程序是2套代码维护，如果基于之前2套代码，分别进行接口替换，需要双倍开发工作量
2、建议使用一套代码重构极速版M和极速小程序

## SKU

预售: 100014196674

百亿补贴: 10037574481980

秒杀: 69498268540

到店LOC: 100022459808