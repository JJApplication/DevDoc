# Twig
服务间通信的桥梁， 基于UDS通信的服务间消息转发模块

## 功能
`Twig`分为服务端和客户端，负责微服务之间的消息传递

服务端负责接受来自其他微服务的数据

客户端负责将数据包转发到指定微服务

### 报文模型
模型由`Fushin`定义
```go
			err := c.Response(uds.Res{
				Error: errors.New(ErrNoFrom).Error(),
				Data:  "",
				From:  OctopusTwig,
				To:    nil,
			})
```
定义了**标准错误**， **数据**，**数据源**， **目的地**

## 特性
- **可压缩的报文**

    默认的报文数据基于JSON序列化, 为尽量减少体积提高效率你可以使用`yaml`, `gob`(go内置的编码格式), `protobuf`对数据进行编码
- **可复用的链接池**

    对于需要经常通信的服务，内部会维护一个链接池用于复用。在服务间通信不频繁时会将uds链接从池中销毁重新创建
- **多目的地转发**

    可以配置多个目的微服务达到一次消息广播发布


## 配置
```bash
export SocketRoot=/tmp
export UnixSocketAddress=/tmp/OctopusTwig.sock
```

`SocketRoot` 全局的微服务UDS通信存储路径

`UnixSocketAddress` twig的UDS存储路径