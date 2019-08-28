
- [ ] 订单
- [ ] 支付
- [ ] 活动

1. 订单生成接口
```shell
实际处理方法
Common\Controller\OfficepayController

orderV50
order

调用位置
路由 http://office.balance.desktop.zhuazi.com/pay/qrcode

Desktop\Controller\PayController     =>  qrcode()
Office\Controller\PayController      =>  qrcode()
Weixinapi\Controller\PayController   =>  qrcode()

请求参数
1. 已登录
token: 8e310ddcc3ec855ecdd3246d7f9ecd13
product_token: 32f8e929a4a7f693db45f8dac5cd745a
buy_position: website
two_purchase: 1
api_version: 2.0
2. 未登录
product_token: 02804067beae3da1faa4ff72edf33f20
buy_position: website
two_purchase: 1
api_version: 2.0

```

2. 支付状态回调
```shell
未登录状态
http://office.balance.desktop.zhuazi.com/order/notLoginQueryOrder
请求参数
order_token: f99252eea2bf7ba31eff9a240505d5b3
api_version: 2.0
login_from: notlogin
```