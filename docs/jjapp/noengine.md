# NoEngine

**NoEngine**是一个基于openresty开发的网络入口

主要负责JJAPP
- 域名绑定
- 代理转发
- 负载均衡
- 静态伺服
- 请求拦截
- 请求鉴权

基于openresty的docker容器部署，使用lua进行功能扩展

很多需要的特性和功能需要额外加载模块这就导致了其作为统一流量入口的功能大大被限制

所以后续将使用`RainbowBridge`服务替代