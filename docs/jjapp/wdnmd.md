# WDNMD
后台服务的监控守护模块， 用于守护当前服务器上需要监控的进程与微服务， 在异常时发送告警通知

## 依赖部件
- Twig
- Hermes
- Apollo

## 配置
```bash
export UnixAddress=/tmp/Wdnmd.sock
export Talker=/tmp/OctopusTwig.sock
export ExtraApps="sshd"
export To=xxx@gmail.com
export MongoName=ApolloMongo
export MongoURL=mongodb://127.0.0.1:27017
```
在环境变量中配置需要监控的进程与服务

修改`ExtraApps`以空格分隔配置自己要监控的进程名称

### 微服务监控
默认依赖`Apollo`的数据库，从中读取注册在`Apollo`的微服务

根据`Octopus`模型排除前端服务, 测试服务, 未发布服务

## 告警流程
- 定时任务触发，检查监控的服务和进程
- 进程白名单对比
- 基于内置模板生成告警文本
- 发送通知到Hermes
- Hermes接收到请求，发送告警邮件到指定地址