# Dubbo #
1. 通信原理![](https://i.imgur.com/xJdEgYF.png)
2. 注册中心挂了dubbo还能用吗？

	> 可以，因为刚开始初始化的时候，消费者会将提供者的地址等信息拉取到本地缓存，所以注册中心挂了可以继续通信
