# Sandwich

意为**三明治**， 即工作在顶层入口和下层微服务之间的网关

## 代理转发

**Sandwich**主要的功能是根据请求的域名，动态代理转发到后端微服务

同时会请求和响应可以被**Sandwich**处理，完成流量统计，限流，熔断，缓存等功能

### 动态转发模型

在数据库中记录了*服务与随机端口对照表*，告诉**Sandwich**域名对应的服务应该转发到哪里

例如：

```json
{
    "app_name": "Blog",
    "domain": "blog.renj.io",
    "run_data": {
        "port": 10000
    }
}
```

当请求`HOST`为blog.renj.io时，对应的服务**Blog**会被匹配，**Sandwich**将转发请求到端口`10000`的服务上

## 错误网关

**Sandwich**定义了一系列异常状态码，在请求超时，请求频率异常，代理后端异常时就会返回统一的错误页面

![sandwich](../media/sandwich.jpg)

## 运行时依赖

**Sandwich**与**Apollo**服务相关联，在**Apollo**服务对微服务模型解析完毕后，经过它启动的服务会被赋予*随机的动态端口*

数据会被**Apollo**记录到`mongo`数据库中，所以**Sandwich**依赖mongo数据库来读取记录的*服务与随机端口对照表*

------

`influxDB`作为可选依赖用于记录基于时序的流量访问数据

## 使用

```bash
➜  Sandwich git:(main) ✗ ./sandwich -h
Usage of ./sandwich:
  -db string
        mongo db name
  -debug
        debug mode
  -enable
        enable influx
  -influx string
        influx db url
  -mongo string
        mongo db url
  -port string
        port
  -size int
        cache size[mb] (default 10)
  -t int
        auto sync time (default 60)
  -token string
        influx db token
```
